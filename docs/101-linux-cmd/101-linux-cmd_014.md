# `mkdir`命令

`mkdir`命令用于在 Linux/Unix 系统中创建目录（文件夹）。它是最基本的文件系统命令之一，提供了创建单个目录、多个目录和嵌套目录结构的各种选项。

## 语法

```sh
      mkdir [OPTIONS] DIRECTORY [DIRECTORY ...] 

```

## 关键特性

+   **单个目录创建**：创建单个目录

+   **多目录创建**：一次创建多个目录

+   **嵌套目录创建**：自动创建父目录

+   **权限设置**：在创建时设置目录权限

+   **详细输出**：显示创建进度

+   **SELinux 支持**：设置安全上下文

## 基本用法

### 创建单个目录

```sh
      # Create a directory in current location
mkdir mydir

# Create directory with absolute path
mkdir /home/user/documents

# Create directory in home directory
mkdir ~/projects 

```

### 创建多个目录

```sh
      # Create multiple directories at once
mkdir dir1 dir2 dir3

# Create directories with different paths
mkdir ~/documents ~/downloads ~/pictures

# Create numbered directories
mkdir project{1,2,3,4,5}

# Create directories with ranges
mkdir folder{01..10} 

```

## 高级用法

### 创建嵌套目录

```sh
      # Create nested directory structure (creates parent directories)
mkdir -p projects/web/frontend/src

# Create complex directory structure
mkdir -p company/{departments/{hr,finance,it},projects/{web,mobile,desktop}}

# Create directory structure with absolute path
mkdir -p /opt/myapp/{bin,config,logs,data} 

```

### 创建时设置权限

```sh
      # Create directory with specific permissions (755)
mkdir -m 755 public_dir

# Create directory with full permissions for owner only (700)
mkdir -m 700 private_dir

# Create directory with read/write for owner, read for group and others (644)
mkdir -m 644 shared_dir

# Create directory with full access for everyone (777)
mkdir -m 777 temp_dir

# Using symbolic notation
mkdir -m u=rwx,g=rx,o=rx public_folder 

```

### 详细模式

```sh
      # Show what directories are being created
mkdir -v newdir

# Verbose with multiple directories
mkdir -v dir1 dir2 dir3

# Verbose with nested structure
mkdir -pv projects/{frontend/{src,dist},backend/{api,database}} 

```

## 实际示例

### 项目结构创建

```sh
      # Create a typical web project structure
mkdir -p mywebsite/{css,js,images,includes,admin}

# Create a software project structure
mkdir -p myproject/{src/{main,test},docs,build,config}

# Create a backup directory structure
mkdir -p backups/{daily,weekly,monthly}/{system,database,files} 

```

### 系统管理

```sh
      # Create log directories for an application
sudo mkdir -p /var/log/myapp/{error,access,debug}

# Create configuration directories
sudo mkdir -p /etc/myapp/{conf.d,ssl,keys}

# Create data directories with proper permissions
sudo mkdir -p /var/lib/myapp/data
sudo mkdir -m 750 /var/lib/myapp/secure 

```

### 用户环境设置

```sh
      # Set up user development environment
mkdir -p ~/development/{projects,tools,scripts}
mkdir -p ~/development/projects/{personal,work,opensource}

# Create organization directories
mkdir -p ~/documents/{work,personal,finance,education}
mkdir -p ~/documents/work/{reports,presentations,spreadsheets} 

```

## 与特殊字符一起工作

### 包含空格的目录

```sh
      # Create directory with spaces (use quotes)
mkdir "My Documents"
mkdir 'Project Files'

# Create multiple directories with spaces
mkdir "Dir One" "Dir Two" "Dir Three"

# Using escape characters
mkdir My\ Documents 

```

### 特殊字符

```sh
      # Create directories with special characters
mkdir "data-2024"
mkdir "backup_$(date +%Y%m%d)"
mkdir "temp.$(whoami)"

# Avoid problematic characters
mkdir project_2024  # Better than "project 2024"
mkdir user_data     # Better than "user's data" 

```

## 错误处理和验证

### 常见错误场景

```sh
      # Check if directory exists before creatingif [ ! -d "mydir" ]; then
    mkdir mydir
    echo "Directory created"
else
    echo "Directory already exists"
fi

# Create directory only if parent exists
mkdir subdir  # Fails if current directory doesn't exist
mkdir -p parentdir/subdir  # Creates both if needed 

```

### 安全目录创建

```sh
      # Function to safely create directoriescreate_safe_dir() {
    if mkdir -p "$1" 2>/dev/null; then
        echo "Created directory: $1"
    else
        echo "Failed to create directory: $1"
        return 1
    fi
}

# Usage
create_safe_dir "/path/to/new/directory" 

```

## 与其他命令结合使用

### 目录创建和导航

```sh
      # Create directory and immediately change to it
mkdir myproject && cd myproject

# Create and navigate in one command (function)
mkcd() {
    mkdir -p "$1" && cd "$1"
}
mkcd ~/projects/newproject 

```

### 创建包含文件的目录

```sh
      # Create directory structure and add files
mkdir -p project/{src,docs,tests}
touch project/src/main.py
touch project/docs/README.md
touch project/tests/test_main.py

# Create directory and set up basic files
mkdir website
cd website
mkdir {css,js,images}
touch index.html css/style.css js/script.js 

```

### 批量操作

```sh
      # Create directories from a list
cat > dirlist.txt << EOF
projects/web
projects/mobile
projects/desktop
documents/reports
documents/presentations
EOF

# Create all directories from file
while read dir; do
    mkdir -p "$dir"
done < dirlist.txt 

```

## 权限和所有权

### 创建后设置所有权

```sh
      # Create directory and set ownership
mkdir myapp
sudo chown user:group myapp

# Create with specific permissions and ownership
sudo mkdir -m 755 /opt/myapp
sudo chown user:group /opt/myapp 

```

### 为不同用户创建目录

```sh
      # Create user-specific directories
sudo mkdir -p /home/newuser/{Documents,Downloads,Pictures}
sudo chown -R newuser:newuser /home/newuser
sudo chmod 755 /home/newuser 

```

## SELinux 上下文

### 设置 SELinux 上下文

```sh
      # Create directory with specific SELinux context
mkdir -Z user_home_t user_data

# Create directory and set context afterward
mkdir secure_data
sudo semanage fcontext -a -t httpd_exec_t "/path/to/secure_data(/.*)?"
sudo restorecon -R /path/to/secure_data 

```

## 故障排除

### 常见问题和解决方案

```sh
      # Permission denied
sudo mkdir /restricted/path  # Use sudo for system directories

# Parent directory doesn't exist
mkdir -p path/to/deep/directory  # Use -p flag

# Directory already exists
mkdir -p existing_dir  # -p prevents error if directory exists

# Invalid characters in name
mkdir "valid_name"  # Use quotes or escape special characters 

```

### 调试目录创建

```sh
      # Check available space before creating
df -h .

# Verify parent directory permissions
ls -ld parent_directory

# Check if directory was created successfully
if [ -d "newdir" ]; then
    echo "Directory created successfully"
    ls -ld newdir
fi 

```

## 选项参考

| **选项** | **长格式** | **描述** |
| :-- | :-- | :-- |
| `-m MODE` | `--mode=MODE` | 为创建的目录设置文件模式（权限） |
| `-p` | `--parents` | 如有必要创建父目录，如果已存在则无错误 |
| `-v` | `--verbose` | 为每个创建的目录打印消息 |
| `-Z CTX` | `--context=CTX` | 设置 SELinux 安全上下文 |
| `--help` | - | 显示帮助信息并退出 |
| `--version` | - | 输出版本信息并退出 |

## 最佳实践

### 目录命名规范

```sh
      # Use descriptive names
mkdir user_documents    # Good
mkdir stuff            # Poor

# Use consistent naming patterns
mkdir project_2024_01
mkdir project_2024_02

# Avoid spaces and special characters
mkdir my-project       # Good
mkdir "my project"     # Works but can cause issues 

```

### 组织策略

```sh
      # Date-based organization
mkdir -p archives/$(date +%Y)/{01..12}

# Project-based organization
mkdir -p projects/{active,completed,archived}

# User-based organization
mkdir -p users/{admins,developers,testers} 

```

### 自动化和脚本

```sh
      #!/bin/bash# Script to create standard project structure

PROJECT_NAME="$1"
if [ -z "$PROJECT_NAME" ]; then
    echo "Usage: $0 <project_name>"
    exit 1
fi

echo "Creating project structure for: $PROJECT_NAME"
mkdir -p "$PROJECT_NAME"/{
    src/{main,test},
    docs/{api,user},
    config/{dev,prod,test},
    scripts/{build,deploy},
    data/{input,output,temp}
}

echo "Project structure created successfully!"
tree "$PROJECT_NAME" 

```

## 与其他工具集成

### 使用版本控制

```sh
      # Create project with Git initialization
mkdir myproject
cd myproject
git init
mkdir {src,docs,tests}
touch .gitignore README.md 

```

### 使用 Docker

```sh
      # Create Docker project structure
mkdir -p docker-project/{app,data,logs,config}
touch docker-project/Dockerfile
touch docker-project/docker-compose.yml 

```

## 重要注意事项

+   使用`-p`标志以避免在目录已存在时出错

+   创建系统目录时请注意权限

+   总是使用引号包围包含空格的目录名

+   考虑使用`tree`命令来可视化创建的目录结构

+   `mkdir`命令使用默认权限创建目录，这些权限由 umask 修改

+   创建目录时使用绝对路径

`mkdir`命令对于在 Linux 系统中组织文件和创建目录结构至关重要。

更多详细信息，请查看手册：`man mkdir`
