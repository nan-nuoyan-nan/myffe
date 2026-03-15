# myffe - Feature Engineering Framework

myffe 是 "My First Feature Engine" 的缩写，一个工业级的特征工程框架，提供完整的特征工程工具链，支持数据清洗、特征提取、样本处理等功能。

## 特性

- **模块化设计**：易于扩展和定制
- **多种预处理方法**：支持缺失值处理、异常值处理、标准化、归一化等
- **预定义路线**：提供 20+ 种预定义的特征工程路线
- **自动参数推荐**：智能推荐最佳预处理参数
- **工业级封装**：外部接口简洁易用
- **纯 Python 实现**：无需编译，跨平台兼容
- **完整的日志系统**：详细记录处理过程
- **数据评估功能**：评估数据质量和处理效果
- **可视化支持**：路线可视化和数据描述

## 安装

```bash
pip install myffe
```

## 快速开始

### 简单使用

```python
import pandas as pd
from myffe import process_data

# 读取数据
data = pd.read_csv('your_data.csv')

# 使用预定义路线 1（基础路线）处理数据
processed_data = process_data(data, data_label='label', route_number=1)
```

### 高级使用

```python
from myffe import FFE

# 创建 FFE 实例
ffe = FFE()

# 创建流水线
console, tools = ffe.create_pipeline('label', route_number=3)

# 处理数据
console(tools, data, save_path='processed_data.csv')
```

### 自动推荐路线

```python
from myffe import FFE

ffe = FFE()

# 自动检测数据并推荐路线
recommended_route = ffe.auto_detect_route(data)

# 使用推荐的路线处理数据
console, tools = ffe.create_pipeline('label', route=recommended_route)
console(tools, data, save_path='processed_data.csv')
```

## 预定义路线

myffe 提供 20+ 种预定义路线，适用于不同场景：

1. **基础路线** - 适合大多数数据集
2. **股票数据路线** - 专为股票数据优化
3. **文本数据路线** - 包含文本处理
4. **时间序列路线** - 强化时间特征提取
5. **完整路线** - 包含所有预处理步骤
6. **轻量级路线** - 快速处理小数据集
7. **异常检测路线** - 专注于异常值处理
8. **特征增强路线** - 强化特征工程
9. **分类数据路线** - 适合分类任务
10. **回归数据路线** - 适合回归任务
11. **中文文本路线** - 专为中文文本数据优化
12. **不平衡数据路线** - 处理类别不平衡的数据集
13. **高维数据路线** - 处理特征维度较高的数据集
14. **金融数据路线** - 专为金融数据优化
15. **医疗数据路线** - 适合医疗数据集的处理
16. **电商数据路线** - 专为电商数据优化
17. **社交媒体数据路线** - 处理社交媒体数据
18. **传感器数据路线** - 处理传感器数据
19. **多模态数据路线** - 处理包含多种类型数据的数据集
20. **实时数据路线** - 适合实时数据流处理

查看可用路线：

```python
from myffe import show_available_routes
show_available_routes()
```

## 核心模块

### 1. 数据预处理 (preprocessing)

- **nan\_preprocessing** - 缺失值处理
  - 支持多种填充策略：均值、中位数、众数、固定值、前向填充、后向填充、KNN填充、智能填充、集成填充(EI)等
  - 支持按列自定义填充方法
  - 支持缺失值比例统计和可视化
  - 智能推荐填充方法，根据数据类型和单调性自动选择最佳填充策略
  - 集成填充(EI)：通过集成多种填充策略（均值、中位数、众数）和多项式回归模型，对预测结果进行加权平均，提供更准确的填充结果。采用验证集评估各填充方法的性能，并根据性能分配不同权重，优化预测准确性。实现了智能迭代次数调整，从20-100之间随机选择初始迭代次数，并根据验证分数自动调整，确保在合理范围内（5-1000次）找到最佳结果。支持多项式阶数设置，可根据数据维度智能推荐：特征数量较少时使用较低阶数（2-3），特征数量较多时使用较高阶数（3-4），数据量特别大时（5000行以上）可适当增加阶数。特别适用于数据量充足（1000行以上）、特征数量适中（编码后不超过30个特征，原始列数量达到3个以上）、缺失比例合理（整体缺失行比例小于30%，目标列缺失比例在5%-40%之间）的数据集。支持自动推荐和手动选择，是处理复杂缺失值场景的强大工具
- **outlier\_preprocessing** - 异常值处理
  - 支持多种异常值检测方法：IQR、Z-score、Isolation Forest等
  - 支持异常值替换或删除
  - 支持异常值统计和可视化
- **str\_preprocessing** - 字符串处理
  - 支持文本清洗、分词、编码
  - 支持类别特征的one-hot编码
  - 支持文本特征提取
- **time\_preprocessing** - 时间特征提取
  - 支持日期时间解析
  - 支持时间特征衍生：年、月、日、时、分、秒等
  - 支持时间差计算
- **standard\_preprocessing** - 数据标准化
  - 支持Z-score标准化
  - 支持Min-Max标准化
  - 支持Robust标准化
  - 支持QuantileTransformer标准化
  - 支持均值归一化
  - 支持单位长度归一化
  - 支持自定义缩放
- **normalized\_preprocessing** - 数据归一化
  - 支持L1、L2归一化
  - 支持MaxAbs归一化
  - 支持Robust归一化
  - 支持幂次归一化
  - 支持对数归一化
  - 支持平方根归一化
- **sample\_preprocessing** - 样本处理
  - 支持数据采样
  - 支持类别平衡处理
  - 支持数据分割
- **relative\_preprocessing** - 相对特征处理
  - 支持特征间的相对计算
  - 支持比例特征创建
- **specify\_preprocessing** - 指定特征处理
  - 支持自定义特征处理逻辑
  - 支持特征选择和变换

### 2. 路线配置 (route)

- **predefined\_routes** - 预定义路线
  - 提供 20+ 种预定义的特征工程路线
  - 支持路线参数获取和可视化
- **auto\_route** - 自动路线推荐
  - 基于数据特征自动推荐最佳处理路线
  - 支持自定义推荐策略
- **route\_config** - 路线配置
  - 支持路线参数的配置和管理
  - 支持路线的自定义和扩展

### 3. 工具 (tools)

- **datadescription** - 数据描述
  - 提供详细的数据统计信息
  - 支持数据质量评估
  - 支持数据分布可视化
- **datainspector** - 数据检查
  - 支持数据类型检查
  - 支持缺失值检测
  - 支持异常值检测
- **visualizeroute** - 路线可视化
  - 支持处理路线的可视化展示
  - 支持处理流程的图形化表示
- **logger** - 日志系统
  - 支持多级别日志
  - 支持控制台和文件日志
  - 支持处理过程的详细记录
  - 支持自定义日志路径
- **color_printer** - 彩色打印
  - 支持彩色输出
  - 支持不同级别的信息展示
  - **限制**：在 Windows 终端中默认禁用颜色输出，以避免编码问题
- **interactive** - 交互式工具
  - 提供交互式数据处理界面
- **evaluation_decorator** - 评估装饰器
  - 用于评估数据处理前后的质量变化
- **input_utils** - 输入工具
  - 提供类型安全的交互式输入功能
  - 支持整数和浮点数输入
  - 自动进行范围验证和错误处理

## 核心接口

### FFE 类

```python
class FFE:
    def __init__(self, log_level='info'):
        """初始化FFE框架
        
        Args:
            log_level (str): 日志级别，可选值: 'debug', 'info', 'warning', 'error'，默认为'info'
        """
    
    def create_pipeline(self, data_label, route_number=None, route=None, **kwargs):
        """创建特征工程流水线
        
        Args:
            data_label (str): 标签列名称，指定数据中的目标变量列
            route_number (int, optional): 预定义路线编号，1-20之间的整数，默认为None
            route (dict, optional): 自定义路线参数字典，由auto_detect_route生成，默认为None
            **kwargs: 其他参数，如batch_size等
        
        Returns:
            tuple: (console, tools) - 控制台实例和预处理工具字典
        """
    
    def auto_detect_route(self, data, label_columns=None, drop_threshold=0.5):
        """自动检测数据并推荐特征工程路线
        
        Args:
            data (DataFrame): 输入数据，pandas DataFrame格式
            label_columns (list, optional): 标签列名称列表，默认为None
            drop_threshold (float, optional): 缺失值阈值，超过此阈值的列会被丢弃，默认为0.5
        
        Returns:
            dict: 推荐的特征工程参数字典
        """
    
    def validate_system(self):
        """验证系统状态
        
        Returns:
            dict: 系统状态验证结果，包含各模块的状态信息
        """
```

### 便捷函数

- \*\*process\_data(data, data\_label, route\_number=None, route=None, save\_path='./processed\_data.csv', **kwargs)**
  - 便捷的数据处理函数，一站式完成数据预处理
  - 参数：
    - data (DataFrame): 输入数据，pandas DataFrame格式
    - data\_label (str): 标签列名称，指定数据中的目标变量列
    - route\_number (int, optional): 预定义路线编号，1-20之间的整数，默认为None
    - route (dict, optional): 自定义路线参数字典，由auto\_detect\_route生成，默认为None
    - save\_path (str): 处理后数据的保存路径，默认为'./processed\_data.csv'
    - \*\*kwargs: 其他参数，如batch\_size等
  - 返回：处理后的数据，pandas DataFrame格式
- **get\_recommended\_route(data, label\_columns=None)**
  - 获取推荐的特征工程路线
  - 参数：
    - data (DataFrame): 输入数据，pandas DataFrame格式
    - label\_columns (list, optional): 标签列名称列表，默认为None
  - 返回：推荐的路线参数字典
- **show\_available\_routes()**
  - 显示可用的预定义路线
  - 返回：路线列表，包含所有可用的预定义路线编号和名称
- **get\_route\_params(route\_number)**
  - 获取指定路线的参数
  - 参数：
    - route\_number (int): 路线编号，1-20之间的整数
  - 返回：路线参数字典，包含该路线的所有预处理步骤和参数
- **describe\_data(data)**
  - 描述数据，生成详细的数据统计信息
  - 参数：
    - data (DataFrame): 输入数据，pandas DataFrame格式
  - 返回：数据描述信息，包含数据基本信息、质量评估、分布情况等
- **evaluate\_dataset(data, label\_columns=None)**
  - 评估数据集质量
  - 参数：
    - data (DataFrame): 输入数据，pandas DataFrame格式
    - label\_columns (list, optional): 标签列名称列表，默认为None
  - 返回：评估指标字典，包含数据完整性、质量、特征质量等多个维度的评估结果
- **visualize\_route(route)**
  - 可视化路线，展示处理流程
  - 参数：
    - route (dict): 路线参数字典，由get\_route\_params或auto\_detect\_route生成
  - 返回：可视化结果，展示路线的处理流程和参数
- **set\_logging\_mode(mode)**
  - 设置全局日志模式
  - 参数：
    - mode (str): 日志模式，可选值: 'None', 'use', 'test'
  - 返回：无
- **set\_logging\_enabled(enabled)**
  - 控制控制台日志输出
  - 参数：
    - enabled (bool): 是否启用控制台输出，True为启用，False为禁用
  - 返回：无
- **set\_logging\_path(log\_path)**
  - 设置自定义日志文件路径
  - 参数：
    - log\_path (str): 自定义日志文件路径
  - 返回：无
- **get\_logging\_status()**
  - 获取当前日志状态
  - 参数：无
  - 返回：包含日志模式、控制台输出状态和日志文件路径的字典

## 使用示例

### 示例 1：基础数据处理

```python
import pandas as pd
from myffe import process_data

# 创建示例数据
data = pd.DataFrame({
    'feature1': [1.0, 2.0, None, 4.0, 5.0],
    'feature2': ['A', 'B', 'A', None, 'C'],
    'label': [0, 1, 0, 1, 0]
})

# 处理数据
processed = process_data(data, data_label='label', route_number=1)
print(processed.head())
```

### 示例 2：自定义路线

```python
from myffe import FFE, get_route_params

# 获取路线 3 的参数
params = get_route_params(3)

# 创建 FFE 实例
ffe = FFE()

# 创建流水线
console, tools = ffe.create_pipeline('label', route=params)

# 处理数据
console(tools, data, save_path='output.csv')
```

### 示例 3：数据评估

```python
from myffe import FFE, evaluate_dataset, print_evaluation_result

ffe = FFE()

# 评估数据集
metrics = evaluate_dataset(data, label_columns=['label'])
print_evaluation_result(metrics)
```

### 示例 4：自动推荐路线

```python
from myffe import FFE, process_data

# 创建示例数据
import pandas as pd
import numpy as np
data = pd.DataFrame({
    'feature1': np.random.randn(100),
    'feature2': np.random.randint(0, 10, 100),
    'feature3': np.random.choice(['A', 'B', 'C'], 100),
    'feature4': np.random.rand(100),
    'feature5': np.random.randn(100),
    'label': np.random.randint(0, 2, 100)
})
# 添加一些缺失值
data['feature1'][::10] = np.nan
data['feature4'][::15] = np.nan

# 获取推荐路线
ffe = FFE()
recommended_route = ffe.auto_detect_route(data, label_columns=['label'])
print(f"推荐路线: {recommended_route}")

# 使用推荐路线处理数据
processed = process_data(data, data_label='label', route=recommended_route)
print(f"处理后数据形状: {processed.shape}")
```

### 示例 5：系统验证

```python
from myffe import FFE

ffe = FFE()

# 验证系统状态
status = ffe.validate_system()
print(f"系统状态: {status}")
```

## 日志系统

myffe 内置日志系统，支持多种日志模式和控制选项：

### 日志模式

- **None模式** - 不记录任何日志
- **use模式** - 生产环境模式，简洁的控制台输出
- **test模式** - 测试模式，详细的日志记录

### 日志控制函数

```python
from myffe import logger, set_logging_enabled, set_logging_mode, get_logging_status, set_logging_path

# 设置日志模式
set_logging_mode('use')  # 设置为生产环境模式
set_logging_mode('test') # 设置为测试模式
set_logging_mode('None') # 禁用日志

# 控制控制台输出
set_logging_enabled(True)  # 启用控制台输出
set_logging_enabled(False) # 禁用控制台输出

# 自定义日志路径
set_logging_path('D:/custom/logs/preprocessing.log')

# 获取当前日志状态
status = get_logging_status()
print(f"当前日志模式: {status['mode']}")
print(f"控制台输出: {status['console_enabled']}")
print(f"日志文件路径: {status['log_path']}")
```

### 日志文件

- **默认路径**：日志文件默认保存在用户主目录下的 `.myffe/logs` 文件夹中，例如：
  - Windows: `C:\Users\用户名\.myffe\logs\preprocessing.log`
  - Linux/macOS: `/home/用户名/.myffe/logs/preprocessing.log`
- **自定义路径**：可以使用 `set_logging_path` 函数自定义日志路径

### 示例：使用不同日志模式

```python
from myffe import FFE, process_data, set_logging_mode, set_logging_enabled
import pandas as pd
import numpy as np

# 创建示例数据
data = pd.DataFrame({
    'feature1': np.random.randn(100),
    'feature2': np.random.randint(0, 10, 100),
    'label': np.random.randint(0, 2, 100)
})

# 测试 None 模式
print("\n=== 测试 None 模式 ===")
set_logging_mode('None')
result = process_data(data, 'label', route_number=1, save_path=None)
print("处理完成，无日志输出")

# 测试 use 模式
print("\n=== 测试 use 模式 ===")
set_logging_mode('use')
set_logging_enabled(True)  # 启用控制台输出
result = process_data(data, 'label', route_number=1, save_path=None)

# 测试 test 模式
print("\n=== 测试 test 模式 ===")
set_logging_mode('test')
set_logging_enabled(True)  # 启用控制台输出
result = process_data(data, 'label', route_number=1, save_path=None)
```

## 系统要求

- Python >= 3.7
- pandas >= 1.3.0
- numpy >= 1.20.0
- scikit-learn >= 0.24.0
- scipy >= 1.6.0
- imbalanced-learn >= 0.8.0 (用于高级采样方法)

## 许可证

GNU General Public License v3.0

## 贡献

欢迎贡献代码！请提交 Issue 或 Pull Request。

## 版本历史

- **1.0.2** - 优化缺失值处理模块，简化代码结构，删除冗余函数（包括drop_high_missing），改进KNN填充和智能填充实现，支持只处理指定列，提高处理效率；优化归一化处理模块和标准化处理模块，增强交互式选择的安全性，确保用户必须选择有效的选项
- **1.0.1** - 优化日志系统，使用用户主目录作为默认日志路径，添加自定义日志路径功能，更新tools模块接口，确保外部接口简洁易用，将`交互式.py`重命名为`interactive.py`，修复编码问题，全面测试验证所有功能
- **1.0.0** - 正式发布版本，包含完整的特征工程工具链
- **3.3.4** - 修复日志系统，添加日志模式控制功能
- **3.3.2** - 修复导入路径和bug
- **3.3.1** - 优化自动路线推荐
- **3.3.0** - 初始版本

## 测试结果

myffe 库经过全面的测试验证，确保所有功能正常运行。测试覆盖以下方面：

- **基础功能测试**：数据预处理、特征提取、样本处理等核心功能
- **高级功能测试**：流水线创建、自动路线推荐、数据评估等
- **工具模块测试**：日志系统、数据描述、路线可视化等
- **异常处理测试**：处理缺失值、异常值、边界情况等
- **性能测试**：处理不同规模数据集的性能

所有测试均已通过，确保库在各种场景下的可靠性和稳定性。

## 联系方式

- Email: <1757175380@qq.com>
- GitHub: [https://github.com/nan-luoyan-nan/myffe](https://github.com/nan-luoyan-nan/myffe)
