```
import re
from bs4 import BeautifulSoup

def create_table(titles, data, html_code, insert_position):
    # 创建HTML页面
    page = BeautifulSoup('<html></html>', 'html.parser')

    # 创建表格
    table = page.new_tag('table')

    # 创建表头
    thead = page.new_tag('thead')
    table.append(thead)
    tr = page.new_tag('tr')
    thead.append(tr)

    # 将标题插入表头中
    for title in titles:
        th = page.new_tag('th')
        th.string = title
        tr.append(th)

    # 创建表格主体
    tbody = page.new_tag('tbody')
    table.append(tbody)

    # 将数据插入表格中
    for row in data:
        tr = page.new_tag('tr')
        tbody.append(tr)
        for cell in row:
            td = page.new_tag('td')
            td.string = cell
            tr.append(td)

    # 查找插入位置
    html_page = BeautifulSoup(html_code, 'html.parser')
    insert_point = html_page.find(string=re.compile(insert_position))

    # 插入HTML代码中
    insert_point.insert_after(table)

    # 返回HTML
    return html_page.prettify()

# 测试函数
html_code = '<body><div id="table-container">Insert position</div></body>'
titles = ['Title 1', 'Title 2', 'Title 3']
data = [['1', '2', '3'], ['4', '5', '6'], ['7', '8', '9']]
print(create_table(titles, data, html_code, 'Insert position'))
```
