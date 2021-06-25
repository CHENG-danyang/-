import time
import tkinter
from tkinter import ttk
from typing import List
from selenium import webdriver
import threading
from Cpython import Init_answer
from Cpython import Find_answer

# 15826461785##123cglxyz
username = "13330237214"
passwd = "Aifay520."
driver = webdriver.Edge()
url = "http://www.attop.com/"
answers = []

with open("E:\\answers.txt", encoding="utf-8") as file:
    for line in file:
        answers.append(line)


def get_section(Class):  # 获取课程章节网址
    sections = []
    for section in driver.find_elements_by_xpath("//dd/ul/li//a"):
        sections.append(section.get_attribute("href"))
    return sections
# def get_classID()


def find_class():
    root = tkinter.Tk()
    root.wm_attributes('-topmost', 1)
    root.title("选择要刷的课程")
    columns = ("课程代码", "课程名称", "成绩", "学习期限", "状态")
    treeview = ttk.Treeview(
        root, height=18, show="headings", columns=columns)
    treeview.column("课程代码", width=60)
    treeview.column("课程名称", width=180)
    treeview.column("成绩", width=60)
    treeview.column("学习期限", width=80)
    treeview.column("状态", width=80)
    treeview.heading("课程代码", text="课程代码")
    treeview.heading("课程名称", text="课程名称")
    treeview.heading("成绩", text="成绩")
    treeview.heading("学习期限", text="学习期限")
    treeview.heading("状态", text="状态")
    driver.get(url+"user/study.htm")
    time.sleep(0.5)
    tables = driver.find_elements_by_xpath(
        "//div[@class='admin_table']/table//tr")
    for index in range(1, len(tables)):
        treeview.bind("<<TreeviewOpen>>",
                      lambda event: check_event(event, treeview))
        treeview.insert('', index-1, values=get_line(tables[index]), tags=tables[index].find_element_by_xpath(
            ".//a").get_attribute("href"))
    treeview.pack()
    tkinter.mainloop()


def check_event(event, treeview):
    select = event.widget.selection()[0]
    start(treeview.item(select)['tags'][0])

def start(classURL):
    driver.get(classURL.replace("index", "learn"))
    for section in get_section(91):  
        driver.get(section)
        time.sleep(0.5)
        evaluate()
        answer()


def evaluate(): 
    buttons = driver.find_elements_by_class_name("BT_ping")
    for index in range(0, len(buttons)):
        buttons = driver.find_elements_by_class_name("BT_ping")
        if buttons[index].text == "已评价":
            continue
        buttons[index].click()
        time.sleep(0.5)
        driver.switch_to.frame(driver.find_element_by_id("pageiframe"))
        driver.find_element_by_class_name("ping_btn_3").click()
        time.sleep(0.5)
        driver.find_element_by_class_name("aui_state_highlight").click()
        time.sleep(0.5)
        driver.switch_to.default_content()
        driver.find_element_by_class_name("aui_close").click()


def init():  # 初始化题库
    with open("E:\\answers.txt", encoding="utf-8") as file:
        flag = 0
        num = 0
        for line in file:
            num = num+1
            if(flag == 1):
                Init_answer(line, num)
                flag = 0
                continue
            if (line == "\n"):
                flag = 1


def logon():  # 登录
    driver.get(url + "user/index.htm")
    driver.switch_to.frame(driver.find_element_by_id("pageiframe"))
    driver.find_element_by_id("username").send_keys(username)
    driver.find_element_by_id("password").send_keys(passwd)
    while True:
        if len(driver.find_elements_by_id("sink_bg")) != 0:
            load()
            break;
        time.sleep(0.2)


def answer():  # 答题系统
    Exes_list = driver.find_elements_by_class_name("un-ans")
    if(len(Exes_list) == 0):
        return
    for index in range(0, len(Exes_list)):  # 所有试题
        text = Exes_list[index].find_element_by_xpath(
            "./p").text  # 题目
        options = Exes_list[index].find_elements_by_xpath("./ul/li")  # 选项
        answer = find_answer(text)
        if (answer == ''):
            continue
        for _index in range(0, len(options)):  # 对于每一个选项
            for x in range(0, len(answer)):  # 对于每一个答案
                if options[_index].text in answer[x]:
                    options[_index].find_element_by_xpath("./*").click()
    driver.execute_script(
        'document.getElementsByClassName("btn100_org")[0].click()')
    driver.find_element_by_class_name("aui_state_highlight").click()


def find_answer(string):  # 在题库中查找答案
    answer = []
    line = Find_answer(string)
    if(line != -1):
        while(answers[line] != "\n"):
            if "#" in answers[line]:
                answer.append(answers[line].replace("#", ""))
            line = line+1
        return answer
    return ''


def load():
    init()
    find_class()


def get_line(table):
    body = table.find_elements_by_xpath("./td")
    values = []
    values.append(body[0].text)
    values.append(body[1].text)
    values.append(body[2].text)
    values.append(body[3].text)
    values.append(body[4].text)
    values.append(body[5].text)
    return values


def main():
    logon()


if __name__ == '__main__':
    main()
