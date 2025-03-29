FROM alpine:3.14

WORKDIR /app

RUN apk add --no-cache nodejs npm

RUN addgroup -S appgroup && adduser -S appuser -G appgroup

COPY ./src/ /app/

RUN npm install

RUN npm run build && \
    npm cache clean --force

RUN npm install -g serve && \
    npm cache clean --force && \
    rm -rf /var/cache/apk/*

RUN chown -R appuser:appgroup /app

USER appuser

EXPOSE ${REACT_APP_PORT}

CMD ["sh", "-c", "serve -s build -l ${REACT_APP_PORT}"]