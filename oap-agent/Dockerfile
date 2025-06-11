FROM python:3.11

ARG GIT_COMMIT
ARG GIT_REPO

RUN curl -LsSf https://astral.sh/uv/install.sh | sh

ENV PATH="/root/.cargo/bin:/root/.local/bin:$PATH"

WORKDIR /app

RUN git clone ${GIT_REPO} .
RUN git checkout ${GIT_COMMIT:-main}

COPY tools-agent.json langgraph.json
COPY agent.py tools_agent/agent.py

RUN uv venv && \
    . .venv/bin/activate && \
    uv pip install -e . && \
    uv pip install --group dev

ENV VIRTUAL_ENV="/app/.venv"
ENV PATH="$VIRTUAL_ENV/bin:$PATH"

EXPOSE ${PORT}

CMD ["uv", "run", "langgraph", "dev", "--no-browser", "--host", "0.0.0.0", "--port", "2024"]
