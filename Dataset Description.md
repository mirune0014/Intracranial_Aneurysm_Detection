データセット説明
コンペティションデータは、CTA、MRA、T1造影後およびT2強調MRIを含むさまざまなモダリティからなる、専門家によって厳選された数千の脳の医療画像シリーズで構成されています。これらのシリーズは、動脈瘤の存在について13箇所でラベル付けされています。すべての動脈瘤の空間位置特定ラベルと、症例のサブセットに対する血管セグメンテーションが含まれています。

ファイルとフィールド情報
train.csv このファイルには主要な訓練ラベルが含まれています。各行は単一の画像シリーズに対応しています。他の列は、動脈瘤の位置のより詳細なラベルおよび基本的な人口統計データを提供します。

SeriesInstanceUID - 各スキャンシリーズの一意の識別子。
Modality - 画像撮影のモードを識別：CTA、MRA、MRIなど。
PatientAge と PatientSex - スキャン対象者の人口統計データ。
Left Infraclinoid Internal Carotid Artery - この箇所における動脈瘤の存在を示す13個の二進変数のうちの1つ。残りの12個が続きます。すべての場合において、1は動脈瘤の存在を示し、0は不在を示します。
Right Infraclinoid Internal Carotid Artery
Left Supraclinoid Internal Carotid Artery
Right Supraclinoid Internal Carotid Artery
Left Middle Cerebral Artery
Right Middle Cerebral Artery
Anterior Communicating Artery
Left Anterior Cerebral Artery
Right Anterior Cerebral Artery
Left Posterior Communicating Artery
Right Posterior Communicating Artery
Basilar Tip
Other Posterior Circulation
Aneurysm Present メインのターゲット変数、二進値で、スキャンのどこかに動脈瘤が存在するかどうかを示します。

train_localizers.csv このファイルは、訓練セットの動脈瘤の位置特定データを提供します。各動脈瘤について、特定の画像（SOPInstanceUID）と動脈瘤の中心付近の座標を提供します。

SeriesInstanceUID - 各スキャンシリーズの一意の識別子、train.csvのものに対応。
SOPInstanceUID - シリーズ内の特定の画像の一意の識別子。
coordinates - 画像内の動脈瘤の中心のxy座標。
location - 動脈瘤の位置のテキスト説明。

series/ 訓練セットのDICOMシリーズを含み、フォルダごとに1つのシリーズがあります。ファイルパスは series/{SeriesInstanceUID}/{SOPInstanceUID}.dcm の形式です。

DICOMファイルには識別子タグが含まれています：PatientID、StudyUID、SeriesUID。3つのタグすべてがDICOM標準への適合のためにDICOMに含まれていますが、SeriesUIDのみが意味を持ちます。PatientIDとStudyUIDタグは何も識別しません。

segmentations/ series/ 内のDICOMシリーズのサブセットに対するNifTI血管セグメンテーションを含みます。ラベルは以下のように説明されます：

ラベル値	ラベル
1	Other Posterior Circulation
2	Basilar Tip
3	Right Posterior Communicating Artery
4	Left Posterior Communicating Artery
5	Right Infraclinoid Internal Carotid Artery
6	Left Infraclinoid Internal Carotid Artery
7	Right Supraclinoid Internal Carotid Artery
8	Left Supraclinoid Internal Carotid Artery
9	Right Middle Cerebral Artery
10	Left Middle Cerebral Artery
11	Right Anterior Cerebral Artery
12	Left Anterior Cerebral Artery
13	Anterior Communicating Artery

kaggle_evaluation/ 評価APIによって使用されるファイル。APIの使用方法については、デモ提出を参照してください。このAPIは、テストセットを一度に1つのシリーズずつ、固定されていないランダムな順序で提出に提供します。テストセットには約2,500のシリーズがあります。

テストセット内のDICOMタグは以下に制限されています：['BitsAllocated', 'BitsStored', 'Columns', 'FrameOfReferenceUID', 'HighBit', 'ImageOrientationPatient', 'ImagePositionPatient', 'InstanceNumber', 'Modality', 'PatientID', 'PerFrameFunctionalGroupsSequence', 'PhotometricInterpretation', 'PixelRepresentation', 'PixelSpacing', 'PlanarConfiguration', 'RescaleIntercept', 'RescaleSlope', 'RescaleType', 'Rows', 'SOPClassUID', 'SOPInstanceUID', 'SamplesPerPixel', 'SliceThickness', 'SpacingBetweenSlices', 'StudyInstanceUID', 'TransferSyntaxUID']。

kaggle competitions download -c rsna-intracranial-aneurysm-detection
