# veo-bot
# Discordで動く動画生成 Bot（APIなし）

from flask import Flask, request, jsonify

app = Flask(__name__)

@app.route("/", methods=["GET"])
def home():
    return "Bot is running!"

@app.route("/api/interactions", methods=["POST"])
def interactions():
    data = request.json
    prompt = data.get("data", {}).get("options", [])[0].get("value", "")
    video_url = f"https://yourdomain.com/video/{prompt}"
    return jsonify({
        "type": 4,
        "data": {
            "content": f"こちらが「{prompt}」の動画です: {video_url}"
        }
    })

if __name__ == "__main__":
    app.run()
📁 app.py ...
from flask import Flask, request, jsonify
import requests

app = Flask(__name__)

@app.route("/", methods=["GET"])
def index():
    return "Bot is running!"

@app.route("/api/interactions", methods=["POST"])
def interactions():
    data = request.json
    option = data.get("data", {}).get("options", [])[0]
    prompt = option.get("value", "")
    video_url = "https://yourdomain.com/video.mp4"
    return jsonify({
        "type": 4,
        "data": {
            "content": f"こちらが「{prompt}」の動画です！\n{video_url}"
        }
    })

if __name__ == "__main__":
    app.run()


📁 requirements.txt ...
flask
requests


📁 .render.yaml ...
services:
  - type: web
    name: veo-bot
    env: python
    buildCommand: pip install -r requirements.txt
    startCommand: python app.py






















