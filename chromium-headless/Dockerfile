# To run:
# docker run -d -p 9222:9222 --cap-add=SYS_ADMIN austinfox/chromium-headless

# Base docker image
FROM debian:stretch-slim
LABEL name="chromium-headless" \
			maintainer="Austin Fox" \
			version="1.0" \
			description="Google Chromium Headless"

RUN apt-get update && apt-get install -y wget --no-install-recommends \
    && wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - \
    && sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list' \
    && apt-get update \
    && apt-get install -y google-chrome-unstable fonts-ipafont-gothic fonts-wqy-zenhei fonts-thai-tlwg fonts-kacst ttf-freefont \
      --no-install-recommends \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get purge --auto-remove -y curl \
    && rm -rf /src/*.deb

# Add chromium as a user
RUN groupadd -r chromium && useradd -r -g chromium -G audio,video chromium \
    && mkdir -p /home/chromium && chown -R chromium:chromium /home/chromium \
		&& chown -R chromium:chromium /opt/google/chromium

# Run chromium non-privileged
USER chromium

# Expose port 9222
EXPOSE 9222

# Autorun chromium headless with no GPU
ENTRYPOINT [ "google-chrome-unstable" ]
CMD [ "--headless", "--disable-gpu", "--remote-debugging-address=0.0.0.0", "--remote-debugging-port=9222" ]
