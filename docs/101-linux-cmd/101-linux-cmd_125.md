# `aplay`命令

`aplay`是用于 ALSA（高级 Linux 声音架构）声卡驱动程序的命令行音频播放器。它支持多种文件格式和多个声卡的多设备。它基本上用于在命令行界面播放音频。`aplay`与`arecord`非常相似，只是它播放而不是录制。对于支持的声音文件格式，采样率、比特深度等可以从声音文件头中自动确定。

## 语法：

```sh
      $ aplay [flags] [filename [filename]] ... 

```

## 选项：

```sh
      -h, –help : Show the help information.
-d, –duration=# : Interrupt after # seconds.
-r, –rate=# : Sampling rate in Hertz. The default rate is 8000 Hertz.
–version : Print current version.
-l, –list-devices : List all soundcards and digital audio devices.
-L, –list-pcms : List all PCMs(Pulse Code Modulation) defined.
-D, –device=NAME : Select PCM by name. 

```

注意：此命令包含我们通常不需要的各种其他选项。如果您想了解更多，只需在您的终端上运行以下命令即可。

```sh
      aplay --help 

```

## 示例：

1.  以 2500hz 频率播放音频仅 10 秒。

    ```sh
              $ aplay -d 10 -r 2500hz sample.mp3 

    ```

    > 以 2500hz 的频率播放 sample.mp3 文件仅 10 秒。

1.  以 2500hz 频率播放完整的音频剪辑。

    ```sh
              $ aplay -r 2500hz sample.mp3 

    ```

    > 以 2500hz 频率播放 sample.mp3 文件。

1.  显示版本信息。

    ```sh
              $ aplay --version 

    ```

    > 显示版本信息。对我来说，它显示为 aplay: 版本 1.1.0

* * *
