FROM dataelement/bisheng-backend:base.v8

WORKDIR /app

# --- 使用阿里云镜像源配置 ---
ENV PIP_INDEX_URL=https://mirrors.aliyun.com/pypi/simple/
ENV UV_PIP_INDEX_URL=https://mirrors.aliyun.com/pypi/simple/
ENV UV_DEFAULT_INDEX=https://mirrors.aliyun.com/pypi/simple/
# -------------------------

COPY ./ ./

# 生成并安装依赖
RUN uv pip compile pyproject.toml --upgrade --output-file requirements.txt && \
    uv pip install -r requirements.txt --system --no-cache-dir && \
    uv cache clean && \
    rm -f requirements.txt


# patch langchain-openai lib. remove this when langchain-openai support reasoning_content
RUN patch -p1 < /app/bisheng/patches/langchain_openai.patch /usr/local/lib/python3.10/site-packages/langchain_openai/chat_models/base.py


CMD ["sh entrypoint.sh"]
