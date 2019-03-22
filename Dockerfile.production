FROM node:10-alpine

RUN addgroup -S pptruser && adduser -S -g pptruser pptruser \
      && mkdir -p /home/pptruser/Downloads \
      && chown -R pptruser:pptruser /home/pptruser

RUN apk update && apk upgrade && \
      echo @edge http://nl.alpinelinux.org/alpine/edge/community >> /etc/apk/repositories && \
      echo @edge http://nl.alpinelinux.org/alpine/edge/main >> /etc/apk/repositories && \
      apk add --no-cache \
      chromium@edge \
      nss@edge \
      freetype@edge \
      harfbuzz@edge \
      ttf-freefont@edge 
# Tell Puppeteer to skip installing Chrome. We'll be using the installed package.
ENV PUPPETEER_SKIP_CHROMIUM_DOWNLOAD true
ENV CHROME_PATH /usr/bin/chromium-browser

# Run everything after as non-privileged user.
USER pptruser
ENV HOST 0.0.0.0
WORKDIR /home/pptruser
COPY package.json package-lock.json ./
RUN npm install
COPY . .
EXPOSE 9000
CMD [ "node", "src/index.js"]