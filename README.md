# Root 方法
- 工具：[Magisk Github](https://github.com/topjohnwu/Magisk) / [KernelSU Github](https://github.com/tiann/KernelSU) / [APatch Github](https://github.com/bmax121/APatch)
- 介紹：[Magisk 官網](https://topjohnwu.github.io/Magisk) / [KernelSU 官網](https://kernelsu.org/) / [APatch 官網](https://apatch.dev)
- 參考：[YouTube](https://www.youtube.com/watch?v=uD6udMEMbPM)
<details>

<summary>方法一：APatch</summary>

## adb command -> 修補 boot.img

You can add text within a collapsed section. 

```powershell
adb reboot bootloader 

fastboot flashing unlock

#刷入修改後的 boot.img 檔案
fastboot flash boot .\apatch_patched_10763_0.10.7_rume.img

#重新啟動
fastboot reboot
```

</details>

<details>
  
<summary>方法二：Magisk、KernelSU (LKM mode)</summary>

## adb command -> 修補 init_boot.img

```powershell
adb reboot bootloader 

fastboot flashing unlock

#刷入修改後的 boot.img 檔案
fastboot flash init_boot .\magisk_patched-27000_WEZ9s.img

#重新啟動
fastboot reboot
```

</details>

<details>
  
  <summary>方法三：KernelSU (GKI mode)</summary>

  ## 手動修補 boot.img

  - 對於某些裝置來說，其 boot.img 格式並不是很常見，不屬於 lz4，gz 和未壓縮；最典型的就是 Pixel，它的 boot.img 格式是 lz4_legacy 壓縮，ramdisk 可能是 gz 也可能是 lz4_legacy 壓縮；此時如果您直接寫入 KernelSU 提供的 boot.img，手機可能無法開機。這時，您可以透過手動修補 boot.img 來完成。
  ### 準備
  - 取得手機的原廠 `boot.img`
  - 下載 KernelSU 提供的與您的裝置 KMI 一致的 [AnyKernel3 Zip](https://github.com/tiann/KernelSU/releases) 檔 [(可參閱使用自訂 Recovery 安裝)](https://kernelsu.org/zh_TW/guide/installation.html#install-with-custom-recovery)。
  - 解壓縮 AnyKernel3 Zip 檔，取得其中的 `Image` 檔，此檔案為具有 KernelSU 的核心。
    
  ### 在 Android 上使用 magiskboot
  
  - 在 Magisk 的 [Release](https://github.com/topjohnwu/Magisk/releases) 頁面 下載最新的 Magisk。
  - 將 `Magisk-*(version).apk` 重新命名為 `Magisk-*.zip` 並解壓縮。
  - 使用 Adb 將 magiskboot 推入至手機：
  ```shell
    adb push Magisk_*/lib/arm64-v8a/libmagiskboot.so /data/local/tmp/magiskboot
  ```
  - 使用 Adb 將原廠 boot.img 和 AnyKernel3 中的 Image 推入至手機。
  ```shell
  adb push boot.img /data/local/tmp/boot.img
  adb push Image /data/local/tmp/Image
  ```
  - adb shell 進入 /data/local/tmp/ 目錄，然後賦予先前推入的檔案可執行權限
  ```shell
  chmod +x magiskboot
  ```
  - adb shell 進入 `/data/local/tmp/` 目錄，執行：
  ```shell
  adb shell
  
  cd /data/local/tmp/
  
  ./magiskboot unpack boot.img
  ```
  - 此時會將 boot.img 解除封裝，得到一個名為 kernel 的檔案，這個檔案是您的原廠核心。
  - 使用 Image 取代 kernel：
  ```shell
  mv -f Image kernel
  ```
  - 執行
  ```shell
  ./magiskboot repack boot.img
  exit
  ```
  - 重新封裝映像，此時您會得到一個 `new-boot.img` 檔案，透過 Fastboot 將這個檔案寫入至裝置即可。
  ```shell
  adb pull /data/local/tmp/new-boot.img ./new-boot.img
  adb reboot bootloader
  
  #刷入修改後的 boot.img 檔案
  fastboot flash boot new-boot.img
  
  #重新啟動
  fastboot reboot
  ```  
</details>
    
# 修補
- [abootloop (預防載入模組後無法正常開機(Magisk 需要))](https://github.com/Magisk-Modules-Alt-Repo/abootloop)
- [ZygiskNext (Zygisk的替代方案，KernelSU、APatch 需要，Magisk 於設定直接安裝 Zygisk)](https://github.com/Dr-TSNG/ZygiskNext)
- [Play Integrity Fix (修補 Google Pay)](https://github.com/chiteroman/PlayIntegrityFix)
- [TrickyStore](https://github.com/5ec1cff/TrickyStore)
- [Tricky-Addon](https://github.com/KOWX712/Tricky-Addon-Update-Target-List)
- [Zygisk-Assistant](https://github.com/snake-4/Zygisk-Assistant)
- [LSPosed](https://github.com/JingMatrix/LSPosed)

# Rooted (Root 之後) 系統更新
## 方法一 Factory flash
- 教學：[參考連結](https://imum.me/posts/googlepixel8pro%E4%B9%8Broot%E5%90%8E%E6%AF%8F%E6%9C%88%E7%B3%BB%E7%BB%9F%E6%9B%B4%E6%96%B0/)
- 下載：[Pixel Factory Images](https://developers.google.com/android/images#husky)
- 注意：修改 falsh-all.bat 刪除 -w(不要刪除資料)
- 更新後按照原 Root 步驟安裝即可
## 方法二 OTA Update (sideload)
- 下載：[Pixel OTA Images](https://developers.google.com/android/ota#husky)
- 刷入：adb sideload ota.zip
- 更新後按照原 Root 步驟安裝即可
# 檢查
- 檢查裝置完整性：[SPIC - Play Integrity Checker](https://play.google.com/store/apps/details?id=com.henrikherzig.playintegritychecker&pcampaignid=web_share)
- 期望值：MEETS_STRONG_INTEGRITY
![圖片](https://github.com/XiaoYu0708/Pixel-8-Pro-Root/blob/main/1727954305455_100.PNG?raw=true)

# 工具
- [DataBackup 資料備份 APP](https://github.com/XayahSuSuSu/Android-DataBackup)
