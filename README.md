# Realtime Video Communication Engine

## 概要
C++とDirectX12を用いた自作エンジン上で、リアルタイム映像通信を行うための通信基盤です。  
対戦ゲームにおいて、離れた相手の映像をゲーム画面上に低遅延で表示し、臨場感のある体験を実現することを目的に開発しています。

本プロジェクトでは、単に映像を送受信するだけでなく、RTT、ジッタ、パケットロス、フレーム欠落、ビットレートなどを計測・可視化し、通信状態を評価しながら安定性を改善できる仕組みを重視しています。

## 技術的な特徴
- Media Foundationによるカメラ映像取得
- JPEG圧縮とUDPによるリアルタイム送信
- 独自プロトコル RNVP によるチャンク分割送信
- frameId / chunkIndex / sequence / sendTime によるパケット管理
- ACKによる欠落検出と欠落チャンクの選択的再送
- ジッタバッファによる到着間隔の揺らぎ吸収
- ImGuiによる通信状態の可視化
- CSV / Markdown形式での実験結果出力

## システム構成
```text
Camera
  ↓
Media Foundation Capture
  ↓
JPEG Encode
  ↓
RNVP Packetize
  ↓
UDP Send
  ↓
UDP Receive
  ↓
Frame Reassembler
  ↓
Jitter Buffer
  ↓
Decode / Texture Upload
  ↓
DirectX12 Rendering
