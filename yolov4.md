# ec2 で yolov4

1. ec2 に入って GPU 有効化
   - source activate tensorflow_p36
2. 学習データを ec2 の'/home/ubuntu/toray/darknet/train'にいれる
   - scp -i ~/.ssh/shimmoto.pem -r /Users/shimmotoshuhei/work/delta/ndis/20006_NDIS/Sagemaker/Jupyter/train ubuntu@175.41.228.160:/home/ubuntu/darknet/cfg/ndis-unit
3. config ファイルを変更('/home/ubuntu/toray/darknet/cfg/train.data')
   - 絶対パスにする
   ```
   classes=1
   train = /home/ubuntu/toray/darknet/data/train/train.txt
   valid = /home/ubuntu/toray/darknet/data/train/test.txt
   names = /home/ubuntu/toray/darknet/cfg/train.names
   backup = /home/ubuntu/toray/darknet/data/train/backup
   ```
4. train.names を変更('/home/ubuntu/toray/darknet/cfg/train.name')
   - ラベル名を入力する
5. 学習開始
   - `./darknet detector train cfg/train.data cfg/train.cfg yolov4.conv.137`
   - `./darknet detector train cfg/ndis-unit/datasets.data cfg/ndis-unit/yolo-obj.cfg /home/ubuntu/darknet/cfg/ndis-unit/initWeight/yolov4.conv.137 -dont_show -mjpeg_port 8090 -map -gpus 0,1,2,3`
## colab で Yolov4 動かす

- 変更するファイル

```convert.py
...
if __name__ == '__main__':
    model_path = 'yolo4_weight.h5'
    anchors_path = 'model_data/yolo4_anchors.txt'
    # coco_calsses.txt の中身（ラベル）を変更
    classes_path = 'model_data/coco_classes.txt'
    # パスを変更
    weights_path = 'ndis-unit-yolov4.weights'
```

- 重み
  - convert.py と同じ位置に配置

## 学習の評価

[mAP,IoU](https://qiita.com/mdo4nt6n/items/08e11426e2fac8433fed)<br>
