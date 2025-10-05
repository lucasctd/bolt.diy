# ---- build stage ----
FROM node:22-bookworm-slim
WORKDIR /app

# Use pnpm
RUN corepack enable && corepack prepare pnpm@9.15.9 --activate
RUN npm install -g wrangler

# Install deps efficiently
COPY package.json pnpm-lock.yaml* ./
RUN pnpm fetch

# Copy source and build
COPY . .
# install with dev deps (needed to build)
RUN pnpm install --offline --frozen-lockfile

# Build the Remix app (SSR + client)
RUN NODE_OPTIONS=--max-old-space-size=4096 pnpm run build

# Keep only production deps for runtime
RUN pnpm prune --prod --ignore-scripts

EXPOSE 5173

CMD ["pnpm", "dockerstart"]