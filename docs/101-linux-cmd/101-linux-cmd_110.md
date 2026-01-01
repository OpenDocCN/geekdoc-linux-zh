# `make` 命令

`make` 命令用于在特定目录结构中自动化多个命令的重用。

例如，在需要更改不同的 Azure 订阅时使用 `terraform init`、`terraform plan` 和 `terraform validate`。这通常按照以下步骤进行：

```sh
      az account set --subscription "Subscription - Name"
terraform init 

```

`make` 命令如何帮助我们，就是它可以一次性自动化所有这些操作：`make tf-init`

### 语法：

```sh
      make [ -f makefile ] [ options ] ... [ targets ] ... 

```

### 示例使用（指南）：

#### 1. 在您的指南目录中创建 `Makefile`

#### 2. 在您的 `Makefile` 中包含以下内容：

```sh
      hello-world:
        echo "Hello, World!"

hello-bobby:
        echo "Hello, Bobby!"

touch-letter:
        echo "This is a text that is being inputted into our letter!" > letter.txt

clean-letter:
        rm letter.txt 

```

#### 3. 执行 `make hello-world` - 这将在我们的终端中输出 "Hello, World"。

#### 4. 执行 `make hello-bobby` - 这将在我们的终端中输出 "Hello, Bobby!"。

#### 5. 执行 `make touch-letter` - 这将创建一个名为 `letter.txt` 的文本文件并在其中添加一行。

#### 6. 执行 `make clean-letter`

### 参考更详细和内容丰富的教程：

（[linoxide - Linux make 命令示例](https://linoxide.com/linux-make-command-examples/)）（[makefiletutorial.com - 名称本身就说明了这一点](https://makefiletutorial.com/)）
