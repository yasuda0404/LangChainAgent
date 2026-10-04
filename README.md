# LangChainAgent

『LangChainによるAIエージェント開発講座』の付属サンプルコード（[Langchain_sample/](Langchain_sample/)）を、Colab環境ではなくローカルで実行できるように環境構築したリポジトリです。

## 必要なもの

- [mise](https://mise.jdx.dev/) （Python / uv のバージョン管理）
- OpenAI / Google AI (Gemini) のAPIキー（使用する章に応じて）

## セットアップ

```bash
mise install
uv sync
```

`mise.toml`でPython 3.12とuvが固定されており、`uv sync`で`pyproject.toml`/`uv.lock`に基づいた仮想環境（`.venv`）が作られます。

### 環境変数の設定

`.env.example`をコピーして`.env`を作成し、使用する章に必要なキーを設定してください。

```bash
cp .env.example .env
```

| 変数名 | 用途 | 取得先 |
| --- | --- | --- |
| `GOOGLE_API_KEY` | Gemini（Google AI Studio）の呼び出し | [Google AI Studio](https://aistudio.google.com/apikey) |
| `OPENAI_API_KEY` | OpenAIモデルの呼び出し | [OpenAI Platform](https://platform.openai.com/api-keys) |
| `TAVILY_API_KEY` | Tavily検索ツール（Chapter2など） | [tavily.com](https://tavily.com) |
| `LANGCHAIN_API_KEY` | LangSmithトレーシング（Chapter7など） | [smith.langchain.com](https://smith.langchain.com) |
| `WIKIMEDIA_API_TOKEN` | Wikipedia検索の高レート制限アクセス（Chapter1） | 下記参照（任意） |

`WIKIMEDIA_API_TOKEN`は未設定でも動作します（匿名アクセス、10リクエスト/分）が、頻繁に使う場合はトークンを設定すると制限が大幅に緩和されます（数千リクエスト/時間）。

1. [meta.wikimedia.org](https://meta.wikimedia.org)でアカウントを作成してログイン
2. `https://meta.wikimedia.org/wiki/Special:OAuthConsumerRegistration/propose/oauth2` にアクセス
3. 「This consumer is for use only by `<ユーザー名>`」にチェック（Owner-only、審査不要で即時発行）
4. **Applicable project** は `*`（全プロジェクト対象）を指定すること — 特定wikiに限定すると認証エラーになります
5. 発行されたアクセストークンを`WIKIMEDIA_API_TOKEN`に設定

## ノートブックの実行

```bash
uv run jupyter notebook Langchain_sample/
```

カーネルは `LangChainAIAgent (uv)` を選択してください（`uv run python -m ipykernel install --user --name langchain-ai-agent --display-name "LangChainAIAgent (uv)"` で登録済み）。

## 内容

| ファイル | 内容 |
| --- | --- |
| [Chapter1.ipynb](Langchain_sample/Chapter1.ipynb) | AIエージェントの基礎、ReActエージェント（OpenAI / Gemini） |
| [Chapter2.ipynb](Langchain_sample/Chapter2.ipynb) | LangChain基礎、チェーン、メモリ、ベクトルストア |
| [Chapter3.ipynb](Langchain_sample/Chapter3.ipynb) | RAG（検索拡張生成） |
| [Chapter7.ipynb](Langchain_sample/Chapter7.ipynb) | LangGraph |
| [Chapter8.ipynb](Langchain_sample/Chapter8.ipynb) | 研究アシスタントエージェント |

書籍付属のオリジナルの説明は[Langchain_sample/README.txt](Langchain_sample/README.txt)を参照してください。

## 備考

サンプルコードはGoogle Colab向けに書かれているため、`google.colab.userdata`によるシークレット取得をローカルの`.env`読み込み（`python-dotenv`）に置き換えています。また、廃止されたモデル（`gpt-3.5-turbo-instruct`、`models/embedding-001`など）の差し替えや、Python 3.12環境での依存関係の調整も行っています。
