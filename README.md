# Patent PDF OCR & Translation Pipeline

英文スキャンPDF（特に特許文書）から、画像前処理と標準LLM（GPT-4.1 / Vision）を組み合わせて高精度にテキストを抽出し、日本語へ翻訳するAI-OCRパイプラインの検証コードです。

## 概要・特徴
本コードは、実務における「精度」「処理速度」「トークンコスト」の課題を解決する設計となっています。

1. **高額なLLM推論コストの削減と精度（ほぼ100%）の両立**
   - 高価なモデルに依存せず、OpenCVを用いた画像前処理（CLAHE、アンシャープ処理、余白トリミング等）を適用することで、標準モデル（GPT-4.1）でも抽出精度「ほぼ100%」を達成。
2. **API並列呼び出しによる処理高速化**
   - `ThreadPoolExecutor` と `Semaphore` を組み合わせ、APIの並列リクエスト制御とレートリミット（スパーティング）最適化を実装。
3. **特許ドメインに特化したプロンプトエンジニアリング**
   - 抽出（Claim_Extraction）および翻訳（Claim_Translation）において、US/JP特許実務の法的ニュアンス（comprising, consisting of 等）を正確に保持するプロンプトを構築。

## 技術スタック
- **言語/環境**: Python 3, Google Colab
- **画像処理/PDF解析**: OpenCV (`cv2`), `pdf2image`, `poppler-utils`
- **AI/LLM**: Azure OpenAI (`gpt-4.1` Vision), LangChain
- **並列処理**: `concurrent.futures`, `threading`

## 実行要件
- Poppler (`apt-get install poppler-utils`)
- 必要なPythonライブラリ: `pip install pdf2image opencv-python openai`
- Azure OpenAI (または OpenAI) のAPIキー設定

※本コードは技術検証用（PoC）のサンプルであり、機密データや実際のファイルパスはダミー化または相対パス化しています。
