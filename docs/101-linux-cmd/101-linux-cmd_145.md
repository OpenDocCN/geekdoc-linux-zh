# `export` 命令

`export` 命令用于设置对子进程可用的环境变量。它使变量对从当前 shell 会话启动的所有进程可用。

## 语法

```sh
      export [options] [variable[=value]]
export [options] [name[=value] ...] 

```

## 环境变量是如何工作的

+   **局部变量**：仅在当前 shell 中可用

+   **环境变量**：对当前 shell 和所有子进程可用

+   `export` 将局部变量转换为环境变量

## 选项

一些流行的选项标志包括：

```sh
      -f    Export functions instead of variables
-n    Remove variable from environment (unexport)
-p    Display all exported variables 

```

## 示例

1.  导出一个简单变量

```sh
      export MY_VAR="Hello World" 

```

1.  一次性导出多个变量

```sh
      export VAR1="value1" VAR2="value2" VAR3="value3" 

```

1.  导出现有的局部变量

```sh
      LOCAL_VAR="test"
export LOCAL_VAR 

```

1.  显示所有导出的变量

```sh
      export -p 

```

1.  导出 PATH 修改

```sh
      export PATH="$PATH:/usr/local/bin" 

```

1.  使用命令替换导出

```sh
      export CURRENT_DATE=$(date)
export HOSTNAME=$(hostname) 

```

1.  取消导出一个变量（从环境中移除）

```sh
      export -n MY_VAR 

```

1.  导出函数

```sh
      my_function() {
    echo "Hello from function"
}
export -f my_function 

```

## 常见环境变量

1.  **PATH** - 可执行搜索路径

```sh
      export PATH="/usr/local/bin:$PATH" 

```

1.  **HOME** - 用户的主目录

```sh
      export HOME="/home/username" 

```

1.  **EDITOR** - 默认文本编辑器

```sh
      export EDITOR="vim"
export VISUAL="code" 

```

1.  **LANG** - 系统语言和区域设置

```sh
      export LANG="en_US.UTF-8" 

```

1.  **PS1** - 主要提示字符串

```sh
      export PS1="\u@\h:\w\$ " 

```

1.  **JAVA_HOME** - Java 安装目录

```sh
      export JAVA_HOME="/usr/lib/jvm/java-11-openjdk" 

```

1.  **NODE_ENV** - Node.js 环境

```sh
      export NODE_ENV="production" 

```

## 开发环境示例

1.  **Python 开发**

```sh
      export PYTHONPATH="$PYTHONPATH:/path/to/modules"
export VIRTUAL_ENV="/path/to/venv" 

```

1.  **Node.js 开发**

```sh
      export NODE_PATH="/usr/local/lib/node_modules"
export NPM_CONFIG_PREFIX="$HOME/.npm-global" 

```

1.  **Go 开发**

```sh
      export GOPATH="$HOME/go"
export GOROOT="/usr/local/go"
export PATH="$PATH:$GOROOT/bin:$GOPATH/bin" 

```

1.  **数据库配置**

```sh
      export DB_HOST="localhost"
export DB_PORT="5432"
export DB_NAME="myapp"
export DB_USER="dbuser" 

```

## Shell 配置文件

通过将它们添加到配置文件中使导出永久化：

1.  **Bash** - `~/.bashrc` 或 `~/.bash_profile`

```sh
      echo 'export MY_VAR="permanent_value"' >> ~/.bashrc 

```

1.  **Zsh** - `~/.zshrc`

```sh
      echo 'export MY_VAR="permanent_value"' >> ~/.zshrc 

```

1.  **系统范围** - `/etc/environment` 或 `/etc/profile`

```sh
      # /etc/environment
MY_GLOBAL_VAR="system_wide_value" 

```

## 检查变量

1.  检查变量是否已导出

```sh
      env | grep MY_VAR
printenv MY_VAR
echo $MY_VAR 

```

1.  检查变量作用域

```sh
      # Local variable
MY_LOCAL="test"
bash -c 'echo $MY_LOCAL'  # Empty output

# Exported variable
export MY_EXPORTED="test"
bash -c 'echo $MY_EXPORTED'  # Shows "test" 

```

## 高级用法

1.  **条件导出**

```sh
      if [ -d "/opt/myapp" ]; then
    export MYAPP_HOME="/opt/myapp"
fi 

```

1.  **使用默认值导出**

```sh
      export EDITOR="${EDITOR:-vim}"
export PORT="${PORT:-3000}" 

```

1.  **导出数组（Bash 4+）**

```sh
      declare -a my_array=("item1" "item2" "item3")
export my_array 

```

1.  **使用验证导出**

```sh
      validate_and_export() {
    if [ -n "$1" ] && [ -n "$2" ]; then
        export "$1"="$2"
        echo "Exported $1=$2"
    else
        echo "Error: Invalid arguments"
    fi
}

validate_and_export "API_KEY" "your-secret-key" 

```

## 用例

+   **开发环境**：设置特定语言的路径

+   **应用程序配置**：数据库 URL、API 密钥、功能标志

+   **系统管理**：自定义 PATH 修改，代理设置

+   **CI/CD 管道**：构建配置，部署目标

+   **安全**：不应在脚本中包含的敏感数据

## 重要提示

+   导出的变量会被子进程继承

+   子进程中导出变量的更改不会影响父进程

+   对于包含空格或特殊字符的值，请使用引号

+   按惯例，环境变量通常是大写

+   在环境变量中处理敏感数据时要小心

+   一些变量（如 PATH）应该追加，而不是替换

## 安全考虑

1.  **避免在导出中包含敏感数据**

```sh
      # Badexport PASSWORD="secret123"

# Better - read from secure file or prompt
read -s -p "Enter password: " PASSWORD
export PASSWORD 

```

1.  **使用临时导出进行敏感操作**

```sh
      # Export temporarilyexport TEMP_TOKEN="secret"
my_command_that_needs_token
unset TEMP_TOKEN  # Clean up 

```

`export` 命令对于 shell 脚本和系统管理至关重要，它使应用程序和进程能够进行适当的环境配置。

更多详细信息，请查看手册：`help export` 或 `man bash`
