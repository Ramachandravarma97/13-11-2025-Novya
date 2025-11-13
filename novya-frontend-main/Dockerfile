# ---- build stage ----
FROM node:22-alpine AS build

WORKDIR /app
COPY package*.json ./

# Use npm install because the lockfile is out of sync and some deps require Node >=20
# Also skip audits/fund for faster CI images.
RUN npm install --no-audit --no-fund

# Accept API URLs at build time
ARG DJANGO_API=http://localhost:8000/api
ARG FASTAPI_API=http://localhost:8001
ENV DJANGO_API=$DJANGO_API
ENV FASTAPI_API=$FASTAPI_API

# Copy the rest of the app
COPY . .

# Replace hardcoded frontend API URLs **inside the image only**
# Adjust paths/patterns if your src/config/api.js uses different literals.
RUN sed -i "s#http://localhost:8000/api#${DJANGO_API}#g" src/config/api.js \
 && sed -i "s#http://localhost:8001#${FASTAPI_API}#g" src/config/api.js

# Build production bundle
RUN npm run build

# ---- runtime stage ----
FROM nginx:alpine
COPY --from=build /app/build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx","-g","daemon off;"]

