# 如何通过云形成（CloudFormation）来增加 EC2 中 EBS 卷的大小。

> 原文：[`techbyexample.com/increase-the-ebs-size/`](https://techbyexample.com/increase-the-ebs-size/)

**1\. 将 EBS 附加到 EC2：**

有多种方法可以将 EBS 附加到 EC2。你可以通过 AWS 控制台 UI 手动将 EBS（块设备）附加到 EC2。你也可以通过终端中的 AWS CLI 来完成这项操作。

**这里，我以 AWS CloudFormation 为例，演示如何创建并将 EBS 附加到 EC2。**

我们已经使用 CloudFormation 在 EC2 上附加了一个 EBS（块设备）。请参阅下面的 CloudFormation 代码。我们有一个映射部分。在资源部分，我们只需在 **LaunchConfig** 资源类型中创建一个 **BlockDeviceMappings**。

```go
"Mappings":{
     "InstanceVolumeSize" : {
            "Production" : {
                "VolumeSize" : "100"    #This is the size of EBS in GB
            }
    }

"Resources": {
        "testLaunchConfig": {
            "Type": "AWS::AutoScaling::LaunchConfiguration",
            "Properties": {
                "BlockDeviceMappings" : [
                    {
                           "DeviceName" : "/dev/sda1",                        
                            "Ebs" : {
                            "VolumeSize" : {
                                "Fn::FindInMap" : [ "InstanceVolumeSize", "Production", "VolumeSize" ]
                            },
                            "VolumeType" : "gp2",
                            "DeleteOnTermination": "true"
                        }
                    }
                ]
      }
}
```

**2\. 调整卷大小，以便它可以扩展到新的分配大小。**

在启动/机器启动时，你可以将以下脚本作为用户数据传递，或者你可以 **在启动时运行此脚本**。请注意，确保在下面的脚本中保持设备名称与在上面 CloudFormation 脚本中提到的 **“/dev/sda1”** 相同。假设下面的脚本名为 “**resize.sh**”。

```go
#!/bin/bash                 
sudo apt install cloud-guest-utils
sudo growpart /dev/xvda 1
sudo resize2fs /dev/xvda1
```

+   [aws](https://techbyexample.com/tag/aws/)*   [ebs](https://techbyexample.com/tag/ebs/)*   [size](https://techbyexample.com/tag/size/)
