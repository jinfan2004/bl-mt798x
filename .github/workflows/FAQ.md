#360T7
name: Build MT7981 U-Boot 360T7 SPI-NAND Support 108M Layout
on:
  push:
    branches: [ master ]
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout source & submodules
        uses: actions/checkout@v4
        with:
          fetch-depth: 0
          submodules: recursive

      - name: Install official required dependencies
        run: |
          sudo apt update
          sudo apt install -y gcc-aarch64-linux-gnu build-essential flex bison libssl-dev device-tree-compiler qemu-user-static

      # 开启MULTI_LAYOUT兼容108M大分区固件
      - name: Compile U-Boot
        run: SOC=mt7981 BOARD=360t7 MULTI_LAYOUT=1 ./build.sh

      - name: Upload compiled uboot artifact
        uses: actions/upload-artifact@v4
        with:
          name: 360T7-UBOOT-MultiLayout-108M
          path: ./output/
          retention-days: 30

#cmcc rax3000m
name: Build MT7981 U-Boot CMCC RAX3000M NAND
on:
  push:
    branches: [ master ]
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout source & submodules
        uses: actions/checkout@v4
        with:
          fetch-depth: 0
          submodules: recursive

      # 完全照搬官方文档的安装命令
      - name: Install official required dependencies
        run: |
          sudo apt update
          sudo apt install -y gcc-aarch64-linux-gnu build-essential flex bison libssl-dev device-tree-compiler qemu-user-static

      # 对齐官方cmcc_rax3000m-emmc示例，不添加MULTI_LAYOUT
      - name: Compile U-Boot
        run: SOC=mt7981 BOARD=cmcc_rax3000m ./build.sh

      - name: Download compiled uboot
        uses: actions/upload-artifact@v4
        with:
          name: CMCC-RAX3000M-NAND-UBOOT
          path: ./output/
          retention-days: 30
