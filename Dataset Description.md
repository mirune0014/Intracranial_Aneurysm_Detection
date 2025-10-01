# データセット説明

コンペティションデータは、**CTA、MRA、T1造影後およびT2強調MRI**を含むさまざまなモダリティからなる、専門家によって厳選された**数千の脳の医療画像シリーズ**で構成されています。

## データセット概要
- **ラベル箇所**: 動脈瘤の存在について13箇所
- **位置情報**: すべての動脈瘤の空間位置特定ラベル
- **血管情報**: 症例のサブセットに対する血管セグメンテーション

## ファイルとフィールド情報

### `train.csv`
主要な訓練ラベルが含まれるファイル。各行は単一の画像シリーズに対応。

#### 基本情報
| フィールド | 説明 |
|-----------|------|
| `SeriesInstanceUID` | 各スキャンシリーズの一意の識別子 |
| `Modality` | 画像撮影のモード（CTA, MRA, MRI等） |
| `PatientAge` | 患者年齢 |
| `PatientSex` | 患者性別 |

#### 動脈瘤位置ラベル（13箇所）
> **注意**: すべて二進変数（1=動脈瘤あり, 0=動脈瘤なし）

| 位置 | 英語名 |
|------|--------|
| 左錐体下内頸動脈 | Left Infraclinoid Internal Carotid Artery |
| 右錐体下内頸動脈 | Right Infraclinoid Internal Carotid Artery |
| 左錐体上内頸動脈 | Left Supraclinoid Internal Carotid Artery |
| 右錐体上内頸動脈 | Right Supraclinoid Internal Carotid Artery |
| 左中大脳動脈 | Left Middle Cerebral Artery |
| 右中大脳動脈 | Right Middle Cerebral Artery |
| 前交通動脈 | Anterior Communicating Artery |
| 左前大脳動脈 | Left Anterior Cerebral Artery |
| 右前大脳動脈 | Right Anterior Cerebral Artery |
| 左後交通動脈 | Left Posterior Communicating Artery |
| 右後交通動脈 | Right Posterior Communicating Artery |
| 脳底動脈先端 | Basilar Tip |
| その他後方循環 | Other Posterior Circulation |

#### メインターゲット
- **`Aneurysm Present`**: **主要ターゲット変数**（スキャンのどこかに動脈瘤が存在するか）

### `train_localizers.csv`
訓練セットの動脈瘤位置特定データを提供。

| フィールド | 説明 |
|-----------|------|
| `SeriesInstanceUID` | スキャンシリーズの識別子（train.csvに対応） |
| `SOPInstanceUID` | シリーズ内の特定画像の識別子 |
| `coordinates` | 画像内の動脈瘤中心のxy座標 |
| `location` | 動脈瘤位置のテキスト説明 |

### `series/` ディレクトリ
**構造**: `series/{SeriesInstanceUID}/{SOPInstanceUID}.dcm`

- 訓練セットのDICOMシリーズを格納
- フォルダごとに1つのシリーズ

#### DICOMタグについて
| タグ | 用途 |
|-----|------|
| `SeriesUID` | **有意味** - 実際の識別に使用 |
| `PatientID` | DICOM標準準拠のためのみ（識別用途なし） |
| `StudyUID` | DICOM標準準拠のためのみ（識別用途なし） |

### `segmentations/` ディレクトリ
`series/`内のDICOMシリーズのサブセットに対する**NifTI血管セグメンテーション**

#### ラベル値対応表
| ラベル値 | 動脈名 |
|---------|--------|
| 1 | Other Posterior Circulation |
| 2 | Basilar Tip |
| 3 | Right Posterior Communicating Artery |
| 4 | Left Posterior Communicating Artery |
| 5 | Right Infraclinoid Internal Carotid Artery |
| 6 | Left Infraclinoid Internal Carotid Artery |
| 7 | Right Supraclinoid Internal Carotid Artery |
| 8 | Left Supraclinoid Internal Carotid Artery |
| 9 | Right Middle Cerebral Artery |
| 10 | Left Middle Cerebral Artery |
| 11 | Right Anterior Cerebral Artery |
| 12 | Left Anterior Cerebral Artery |
| 13 | Anterior Communicating Artery |

### `kaggle_evaluation/` ディレクトリ
**評価API用ファイル**

#### 特徴
- **使用方法**: [デモ提出](https://www.kaggle.com/code/ryanholbrook/rsna-aneurysm-detection-demo-submission)を参照
- **提供方式**: テストセットを一度に1シリーズずつ、ランダム順序で提供
- **テストセット規模**: 約2,500シリーズ

#### テストセットDICOMタグ制限
<details>
<summary><strong>利用可能なDICOMタグ一覧</strong></summary>

```
BitsAllocated, BitsStored, Columns, FrameOfReferenceUID, 
HighBit, ImageOrientationPatient, ImagePositionPatient, 
InstanceNumber, Modality, PatientID, PerFrameFunctionalGroupsSequence, 
PhotometricInterpretation, PixelRepresentation, PixelSpacing, 
PlanarConfiguration, RescaleIntercept, RescaleSlope, RescaleType, 
Rows, SOPClassUID, SOPInstanceUID, SamplesPerPixel, 
SliceThickness, SpacingBetweenSlices, StudyInstanceUID, TransferSyntaxUID
```
</details>

## データダウンロード

```bash
kaggle competitions download -c rsna-intracranial-aneurysm-detection
