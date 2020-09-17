#fzu-tset
hello world!
import json
import os
import argparse
class data:
    def init(dict):
        # 初始化建表
        if OS.path.exists("data.db"):
            OS.remove("data.db")
        Cer = sqlite3.connect("data.db")
        Cer.execute('''CREATE TABLE GITHUB (
                    user_name   TEXT NOT NULL,
                    repo_name   TEXT NOT NULL,
                    event       TEXT NOT NULL
                    );''')
        Cer.commit()
        for filename in os.listdir(directory):
            if filename.endswith(".json"):
                fullpath = dict + '/' + filename
                with open(fullpath, 'r', encoding='utf-8') as file:
                    for line in file:
                        # 读取 json 文件中一行并暂存有效数据
                        data = json.loads(line)
                        insertData = (data['actor']['login'],
                                      data['repo']['name'], data['type'])
                        sqlInsert = 'INSERT INTO GITHUB(user_name, repo_name, event) VALUES(?, ?, ?)'
                        connector.execute(sqlInsert, insertData)
        Cer.commit()
        print(0)
        Cer.close()
        
if __name__='__main__':
        parser = argparse.ArgumentParser()
        parser.add_argument('-i', '--init', help='Initialize the database with files in the folder given.')
        parser.add_argument('-u', '--user', help='Provide a user name.')
        parser.add_argument('-r', '--repo', help='Provide a repository\'s name.')
        parser.add_argument('-e', '--event', help='Provide a type of events.')
        args = parser.parse_args()
        if (args.init):
            init(args.init)
        else:
            if not OS.path.exists('data.db'):
                raise RuntimeError('Error: Initialization is required.')
            elif args.event:
                if args.user:
                    if args.repo:
                        # 三个参数都提供即为第 3 种查询
                        query(2, user=args.user, repo=args.repo, event=args.event)
                    else:
                        query(0, user=args.user, event=args.event)
                elif args.repo:
                    query(1, repo=args.repo, event=args.event)
                else:
                    raise RuntimeError('Error: Argument -l or -c is required.')
            else:
                raise RuntimeError('Error: Argument -e is required.')
def query(query_type, user="", repo="", event=""):
    Cer = sqlite3.connect("data.db")
    getCount0 = 'SELECT * FROM GITHUB WHERE user_name=? AND event=?'
    getCount1 = 'SELECT * FROM GITHUB WHERE repo_name=? AND event=?'
    getCount2 = 'SELECT * FROM GITHUB WHERE user_name=? AND repo_name=? AND event=?'
    if query_type == 0:
        # 查用户事件数
        value = Cer.execute(getCount0, (user, event))
    elif query_type == 1:
        # 查仓库事件数
        value = Cer.execute(getCount1, (repo, event))
    elif query_type == 2:
        # 查用户在仓库的事件数
        value = Cer.execute(getCount2, (user, repo, event))
    result = len(list(value))
    print(result)
    Cer.close()
return result


