FROM node:16.18

ENV PORT=3000

COPY . /frontend-react-js
WORKDIR /frontend-react-js
RUN npm install
EXPOSE ${PORT}
HEALTHCHECK CMD curl --fail http://localhost:3000 || exit 1
CMD ["npm", "start"]