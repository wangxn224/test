# test
一、 任务一：网页数据采集
目标： 访问 https://www.chinamoney.com.cn/english/bdInfo/，通过程序模拟用户操作，筛选出“Treasury Bond”和“2023年”发行的债券，并解析表格，提取指定列的数据，最终保存为CSV文件。
实现思路：
1.环境与工具： 使用 Python 语言，结合 selenium 库模拟浏览器操作，处理动态加载的网页内容。使用 pandas 库处理和保存数据。
2.自动化流程：
启动 Chrome 浏览器（无头模式，也可取消注释以可视化）。
打开目标网页。
使用 WebDriverWait 和 Select 类，分别定位并选择 Bond Type 下拉框为“Treasury Bond”（值 100001）和 Issue Year 下拉框为“2023”（值 2023）。
点击“Search”按钮，触发数据查询。
等待结果表格加载完成。
使用 BeautifulSoup 解析页面源码，定位表格 (<table>) 并提取表头和所有数据行。
根据题目要求的列名（ISIN, Bond Code, Issuer, Bond Type, Issue Date, Latest Rating），从原始数据中筛选出对应列。
将筛选后的数据存入 pandas.DataFrame，并保存为 bond_data.csv 文件，使用 utf-8-sig 编码以确保中文兼容性。
代码文件： TestOne.py
运行方式：
bash
# 确保已安装必要的库pip install selenium pandas beautifulsoup4 webdriver-manager# 运行脚本python TestOne.py
输出结果：
1.主要输出： bond_data.csv，包含所有符合条件的债券数据。
2.调试文件（仅在程序出错时生成）：
page_debug.html: 出错时的页面源码快照。
debug_screenshot.png: 出错时的页面截图。

二、 任务二：自定义正则匹配函数
目标： 实现一个通用的 reg_search 函数，能够根据传入的正则表达式列表，从文本中灵活地提取信息。
实现思路：
1.函数设计： 函数 reg_search(text, regex_list) 接收两个参数。regex_list 是一个列表，其中每个元素是一个字典，键为字段名，值为对应的正则模式。
2.核心逻辑：
遍历 regex_list 中的每个 regex_dict。
对于每个 regex_dict，遍历其键值对。
如果值的正则模式为特殊标识符 *自定义*，则针对特定字段（如 标的证券、换股期限）执行预设的自定义匹配逻辑。
否则，使用 re.findall() 执行通用的正则匹配。
将匹配结果（单个或多个）存入结果字典。
将每个 regex_dict 对应的结果字典添加到最终的结果列表中。
3.自定义逻辑解析：
标的证券： 使用正则 (\d{6}\.[A-Z]{2}) 匹配类似 600900.SH 的股票代码格式。
换股期限：
首先尝试匹配日期范围模式 (\d{4}\s*年\s*\d{1,2}\s*月\s*\d{1,2}\s*日)\s*至\s*(\d{4}\s*年\s*\d{1,2}\s*月\s*\d{1,2}\s*日)，直接提取起始和结束日期。
如果上述模式不匹配，则退而使用 re.findall 提取文本中所有符合 \d{4}\s*年\s*\d{1,2}\s*月\s*\d{1,2}\s*日 格式的日期。
将提取到的中文字符串转换为 YYYY-MM-DD 的标准日期格式。
代码文件： TestTwo.py
运行方式：
bash
# 直接运行脚本即可看到测试用例的输出python TestTwo.py
输出示例：
python
[{'标的证券': '600900.SH', '换股期限': ['2023-06-02', '2027-06-01']}]
