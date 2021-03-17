FROM python:3.8-alpine

WORKDIR /usr/src/app

# 環境変数を設定
# Pythonがpyc filesとdiscへ書き込むことを防ぐ
ENV PYTHONDONTWRITEBYTECODE 1
# Pythonが標準入出力をバッファリングすることを防ぐ
ENV PYTHONUNBUFFERED 1

# psycopg2のインストール
RUN apk update && \
    apk add --virtual build-deps gcc python3-dev musl-dev && \
    apk add postgresql-dev && \
    pip install psycopg2 && \
    apk del build-deps

COPY requirements.txt /usr/src/app/requirements.txt
RUN pip install --upgrade -r requirements.txt

# ホストのpipfileをコンテナの作業ディレクトリにコピー
COPY Pipfile /usr/src/app/Pipfile

# Pipfile.lockを無視してPipfileに記載のパッケージをシステムにインストール. その後pipenvをアンインストール
RUN pipenv install --system --skip-lock \
    && pip uninstall -y pipenv virtualenv-clone virtualenv

# シェルスクリプトをコピー
COPY entrypoint.sh /usr/src/app/entrypoint.sh

COPY . /usr/src/app

# シェルスクリプトを実行
ENTRYPOINT ["/usr/src/app/entrypoint.sh"]

# CMD [ "python", "./manage.py", "runserver", "0.0.0.0:8000"]
