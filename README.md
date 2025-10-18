# veo-bot
# Discordで動く動画生成 Bot（APIなし）

from flask import Flask, request, jsonify

app = Flask(__name__)

@app.route("/api/interactions", methods=["POST"])
def interactions():
    data = request.json

    # 👇 ここが追加ポイント！DiscordのPINGに返事する
    if data["type"] == 1:
        return jsonify({"type": 1})

    # 👇 ここからはコマンド処理
    option = data.get("data", {}).get("options", [])[0]
    prompt = option.get("value", "")
    video_url = "https://yourdomain.com/videos"
    return jsonify({
        "type": 4,
        "data": {
            "content": f"こちらが{prompt}の"
        }
    })


if __name__ == "__main__":
    app.run()
📁 app.py ...
from flask import Flask, request, jsonify
from nacl.signing import VerifyKey

app = Flask(__name__)

PUBLIC_KEY = "ここにDiscordのPublic Keyを貼る"
verify_key = VerifyKey(bytes.fromhex(PUBLIC_KEY))

@app.route("/api/interactions", methods=["POST"])
def interactions():
    signature = request.headers["X-Signature-Ed25519"]
    timestamp = request.headers["X-Signature-Timestamp"]
    body = request.data.decode("utf-8")

    try:
    verify_key.verify(
        f"{timestamp}{body}".encode(),
        signature=bytes.fromhex(signature)
    )
except:
    return "invalid request signature", 401

data = request.json
if data["type"] == 1:
    return jsonify({"type": 1})

elif data["type"] == 2 and data["data"]["name"] == "test":
    return jsonify({
        "type": 4,
        "data": {
            "content": "こちらが動画です！"
        }
    })

if __name__ == "__main__":
    app.run()


















