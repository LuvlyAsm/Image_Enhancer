# Image Enhancer Backend — Multi-Stage Dockerfile

# ---------- Stage 1: builder ----------
FROM node:20-slim AS builder

WORKDIR /app

# sharp's install step needs these to build/fetch its native binary cleanly
RUN apt-get update && apt-get install -y --no-install-recommends \
    python3 \
    build-essential \
    && rm -rf /var/lib/apt/lists/*

COPY package.json package-lock.json* ./
RUN npm install --production

COPY . .

# ---------- Stage 2: final runtime image ----------
FROM node:20-slim

LABEL maintainer="ASM <luvlyasmr@gmail.com>"
LABEL description="Image Enhancer API - real image processing with sharp"

WORKDIR /app

# 1. Copy files and set ownership directly to 'node'
COPY --chown=node:node --from=builder /app /app

ENV PORT=3000
ENV UPLOAD_DIR=/app/uploads

# 2. Volume Creation

VOLUME ["/app/uploads"]

EXPOSE 3000

# 3. Switch to non-root user
USER node

CMD ["node", "server.js"]
