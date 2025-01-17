# Last modified: Sat, 18 Sep 2021 16:44:10 +0000
FROM demisto/python3-deb:3.9.7.24076

COPY requirements.txt .

RUN apt-get update && apt-get install -y --no-install-recommends \
  python3-dev \
  wget \
  libxss1 \
  libappindicator1 \
  poppler-utils \
  fonts-noto-cjk \
  fonts-noto-core \
  zlib1g-dev \
&& pip3 install --no-cache-dir -r requirements.txt \
&& apt-get purge -y --auto-remove \
  python3-dev \
  zlib1g-dev \
&& rm -rf /var/lib/apt/lists/*

# Based on instructions at: https://tecadmin.net/setup-selenium-with-chromedriver-on-debian/
# Will install the latest version available from the chrome repo.
# To see available chromedriver go to https://chromedriver.storage.googleapis.com/ and search for the major version
# of chrome(for  example 91.0).

COPY download_chromedriver.sh .

# Deps needed by google-chrome
RUN apt-get update && apt-get -t buster-backports install -y --no-install-recommends \
  gnupg1 \
  libgtk-3-0 \
  libu2f-udev \
  udev \
&& apt-get update && apt-get install -y --no-install-recommends \
  curl \
  unzip \
&& curl -sS -o - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - \
&& echo "deb [arch=amd64]  http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list \
&& apt-get -y update && apt-get install -y google-chrome-stable \
&& ./download_chromedriver.sh \
&& unzip chromedriver_linux64.zip \
&& mv chromedriver /usr/bin/chromedriver \
&& rm -rf chromedriver_linux64.zip \
&& google-chrome --version \
&& chromedriver --version \
&& apt-get purge -y --auto-remove \
  curl \
  unzip \
&& rm -rf /var/lib/apt/lists/*
