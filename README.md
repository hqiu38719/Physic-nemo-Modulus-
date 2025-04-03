# Modulus環境建置

教學如何安裝Modulus在本機或是Linux server

**Modulus現已更名為Physic-nemo**

## Requirements 

Modulus需要的環境必須在Linux中，Pypi上的套件都無法使用(或者說是使用差異，目前沒有研究出來Pypi的套件如何使用)，Windows電腦需搭配WSL2使用

- Linux environment
- Nvidia official container
- Disk memory required 30GB 

<!-- Check the [installation notes]() for more details on how to install the project. -->

## Installation

1. 如為Windows，請安裝WSL並檢查是否為WSL2，WSL1應該無法使用

    在CMD中輸入```wsl -l -v```查看Version版本
2. Linux中安裝container
    

   ```docker pull nvcr.io/nvidia/modulus/modulus:24.12```
   
   Modulus版本可以參考[Nvidia Officail Container][1]

   [1]: https://catalog.ngc.nvidia.com/orgs/nvidia/teams/modulus/containers/modulus/tags

3. 安裝好Docker image後，輸入
    ```
    docker run -itd --gpus all --name 容器名 -e NVIDIA_DRIVER_CAPABILITIES=compute,utility -e NVIDIA_VISIBLE_DEVICES=all 鏡像名
    ``` 
    創建Container，這行指令的意思就是創建容器並讓你的容器可以使用GPU。



## Notice

簡述一下我們遇到的問題

1. Modulus版本不同，API用法也會有些許不同，API Function路徑可能自己找一下放在哪裡。

   舉例來說22.09版本要import to_yaml()寫法是
   ```
   from modulus.hydra import to_yaml 
   ```
   而24.12版本則是
   ```
   from modulus.sym.hydra import to_yaml
   ```

2. 我們設備使用的是RTX 2070、RTX 3060，但是使用22.09版本無法調用GPU使用CUDA，推測原因是22.09版本環境下的Pytorch版本無支持CUDA，但是又不能自己升級，因為會破壞環境中其他套件相容性的問題，我們解決方法是安裝最新的Modulus版本(也就是下載新的image，創建新的Container)，建議裝好Container後先確認程式是否能調用GPU

## Useful 

因為WSL預設是裝在系統碟，如果容量不夠想要把WSL裝在電腦中其他硬碟，可以參考[WSL轉移教學][2]

[2]: https://medium.com/@inkfan130924783/%E5%B0%87wsl2-%E7%B3%BB%E7%B5%B1%E8%B3%87%E6%96%99%E5%A4%BE%E7%A7%BB%E8%87%B3%E5%8F%A6%E4%B8%80%E9%A1%86%E7%A1%AC%E7%A2%9F-6f0c383d7a0c
