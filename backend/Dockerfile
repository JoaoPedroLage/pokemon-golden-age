FROM node:20

WORKDIR /app

COPY package*.json ./
COPY prisma ./prisma/
RUN npm ci

COPY . .
RUN npx prisma generate
RUN npm run build

EXPOSE 3001

RUN apt-get update && apt-get install -y wait-for-it && rm -rf /var/lib/apt/lists/*

# Script de inicialização enxuto
RUN echo '#!/bin/sh\n\
set -e\n\
echo "Aguardando PostgreSQL..."\n\
wait-for-it postgres:5432 -t 60\n\
echo "Executando migrations..."\n\
npx prisma migrate deploy\n\
echo "Iniciando servidor..."\n\
exec npm run start:prod' > /app/start.sh && chmod +x /app/start.sh

CMD ["/app/start.sh"]
