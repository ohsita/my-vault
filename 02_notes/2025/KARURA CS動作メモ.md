---
tags:
  - note
---
#### 蛍光
SSH接続で
colcon build
ros2 run v4l2_camera v4l2_camera_node
別ターミナルで
source install/setup.bash
ros2 run fluorescence capture_and_count

ローカルターミナルで
C:/Python38/python.exe c:/Users/daxia/Desktop/URC2025/Fluor_average.py

#### インカメ
WSL Ubuntu 22.04.05 LTSで
SSH karura_science@192.168.137.40
ログインする
./runcam.sh
別ターミナルで
rqt

#### 気圧計
WSL 同じく
SSH ログイン
./runpressure.sh

