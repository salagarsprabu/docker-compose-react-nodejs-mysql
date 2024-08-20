# Stage 1
FROM node:14 as build-stage

WORKDIR /bezkoder-ui

# Install necessary system packages and tools to perform chaos tests
RUN apt-get update && \
    apt-get install -y \
    curl \
    stress \
    iproute2 \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*
    
COPY package.json .
RUN npm install
COPY . .

ARG REACT_APP_API_BASE_URL
ENV REACT_APP_API_BASE_URL=$REACT_APP_API_BASE_URL

RUN npm run build

# Stage 2
FROM nginx:1.17.0-alpine

COPY --from=build-stage /bezkoder-ui/build /usr/share/nginx/html
EXPOSE $REACT_DOCKER_PORT

CMD nginx -g 'daemon off;'
