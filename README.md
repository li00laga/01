name: IPTV All
on:
  push:
branches:
      - main
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2
          - name: Set up Python
        uses: actions/setup-python@v2
        with:
          python-version: 3.x
      - name: Install dependencies
        run: pip install selenium requests
      - name: Install Chrome WebDriver
        run: |
          LATEST_CHROMEDRIVER_VERSION=120.0.6099.109
          curl -sS -o chromedriver_linux64.zip "https://edgedl.me.gvt1.com/edgedl/chrome/chrome-for-testing/120.0.6099.109/linux64/chrome-headless-shell-linux64.zip"
          sudo unzip chromedriver_linux64.zip -d /usr/local/bin
          rm chromedriver_linux64.zip
      - name: Set chromedriver path
        run: |
          sudo ln -sf /usr/local/bin/chrome-headless-shell-linux64/chrome-headless-shell /usr/local/bin/chromedriver
          sudo chmod +x /usr/local/bin/chromedriver
      - name: Run script
        run: python ${{ github.workspace }}/IPTV.py
      - name: 提交更改
        run: |
          git config --local user.email "fenglisli@163.com"
          git config --local user.name "li00laga"
          git add .
          git commit *.txt -m "Add generated file"
          git push -f# 01
