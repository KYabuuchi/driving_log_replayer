# Driving Log Replayer シナリオフォーマット定義

driving_log_replayer で用いるシナリオのフォーマットについて述べる。

## フォーマットに関する注意事項

- キーは CamelCase にて定義する。
- 座標系に関しては、特に指定なければ Lanelet2 と同じ座標系にて記述。マップ座標系。
- 単位系に関しては、特に指定がなければ以下を使用する。

```shell
距離: m
速度: m/s
加速度: m/s^2
時間: s
時刻: UNIX TIME
```

## サンプル

シナリオのサンプルを docs/sample フォルダに格納している。

## フォーマット

基本構造は以下の通り。このフォーマットを元に、以下で説明する型を用いて、具体的なシナリオを記述していく。

```yaml
ScenarioFormatVersion: 2.2.0
ScenarioName: String
ScenarioDescription: String
# vehicle setup
SensorModel: String
VehicleModel: String
VehicleId: String
# map
LocalMapPath: String
Evaluation:
  UseCaseName: String
  UseCaseFormatVersion: String
  Conditions: Dictionary # refer use case
```

### ScenarioFormatVersion

シナリオフォーマットのバージョン情報を記述する。セマンティックバージョンを用いる。
現バージョンは 2.2.0 で、フォーマットの更新の度にマイナーバージョンを更新する。

### ScenarioName

シナリオの名前を記述する。Autoware Evaluator 上でシナリオの表示名として使用される。

### ScenarioDescription

シナリオの説明を記述する。Autoware Evaluator 上でシナリオの説明として使用される。

### SensorModel

autoware_launch/launch/logging_simulator.launch.xml の引数の sensor_model を指定する

### VehicleModel

autoware_launch/launch/logging_simulator.launch.xml の引数の vehicle_model を指定する

### VehicleId

autoware_launch/launch/logging_simulator.launch.xml の引数の vehicle_id を指定する。

車両 ID の指定がない場合は、default を指定する。

### LocalMapPath(2.1.0 で導入)

ローカル環境で使用する地図のフォルダのパスを記述する。

`$HOME`のような環境変数を使用することが出来る。

### Evaluation

シミュレーション結果を評価する条件を定義する。

#### UseCaseName

評価プログラムを指定する。

- 診断機能： performance_diag
- 自己位置： localization

など評価したいユースケースを指定する。

ここで指定された名前と同じ名前の評価用 launch を呼び出すことで評価が実行される。
driving_log_replayer/launch に指定した名称と同じ名称の launch.py ファイルが存在している必要がある。

評価用ノードを追加して、driving_log_replayer/launch に launch ファイルを追加することで、評価できるユースケース数を増やせる。

#### UseCaseFormatVersion

ユースケースのフォーマットのバージョン情報を記述する。セマンティックバージョンを用いる。
メジャーバージョンが 1 になるまでは、フォーマットの更新の度にマイナーバージョンを更新する。初期バージョンは 0.1.0。

シナリオと評価プログラムの両方に format バージョンを埋め込み、評価プログラムとフォーマットのバージョンが一致しているかをチェックする。

#### Conditions

ユースケース毎に設定できる条件を指定する。

指定可能な条件は各ユースケースを参照。