# Root 方法
- 工具：[Magisk Github](https://github.com/topjohnwu/Magisk) / [KernelSU Github](https://github.com/tiann/KernelSU) / [APatch Github](https://github.com/bmax121/APatch)
- 介紹：[Magisk 官網](https://topjohnwu.github.io/Magisk) / [KernelSU 官網](https://kernelsu.org/) / [APatch 官網](https://apatch.dev)
- 參考：[YouTube](https://www.youtube.com/watch?v=uD6udMEMbPM)
- 方法一：adb command -> 修補 boot.img (APatch)
```powershell
adb reboot bootloader 

fastboot flashing unlock

#刷入修改後的 boot.img 檔案
fastboot flash boot .\apatch_patched_10763_0.10.7_rume.img

#重新啟動
fastboot reboot
```
- 方法二：adb command -> 修補 init_boot.img (Magisk、KernelSU (LKM mode))
```powershell
adb reboot bootloader 

fastboot flashing unlock

#刷入修改後的 boot.img 檔案
fastboot flash init_boot .\magisk_patched-27000_WEZ9s.img

#重新啟動
fastboot reboot
```

# 修補
- [ZygiskNext (Zygisk的替代方案，KernelSU、APatch 需要，Magisk 於設定直接安裝 Zygisk)](https://github.com/Dr-TSNG/ZygiskNext)
- [PlayIntegrityFix (修補 Google Pay)](https://github.com/chiteroman/PlayIntegrityFix)
- [TrickyStore](https://github.com/5ec1cff/TrickyStore)
- [TSupport](https://t.me/CitraIntegrityTrick/3)
- [LSPosed_mod](https://github.com/mywalkb/LSPosed_mod)
- [abootloop (預防載入模組後無法正常開機)](https://github.com/Magisk-Modules-Alt-Repo/abootloop)
- [Magisk-iOS-Emoji](https://github.com/Keinta15/Magisk-iOS-Emoji)

# Rooted (Root 之後) 系統更新
- 教學：[參考連結](https://imum.me/posts/googlepixel8pro%E4%B9%8Broot%E5%90%8E%E6%AF%8F%E6%9C%88%E7%B3%BB%E7%BB%9F%E6%9B%B4%E6%96%B0/)
- 下載：[Pixel Factory Images](https://developers.google.com/android/images#husky)
- 注意：修改 falsh-all.bat 刪除 -w(不要刪除資料)
- 更新後按照原 Root 步驟安裝即可

# 檢查
- 檢查裝置完整性：[SPIC - Play Integrity Checker](https://play.google.com/store/apps/details?id=com.henrikherzig.playintegritychecker&pcampaignid=web_share)
- 期望值：MEETS_DEVICE_INTEGRITY
![圖片](https://github.com/XiaoYu0708/Pixel-8-Pro-Root/blob/main/1724427301837_100.png?raw=true)

# 工具
- [MMRL](https://github.com/DerGoogler/MMRL)
