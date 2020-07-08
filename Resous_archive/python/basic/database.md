# 数据库

### MySQL数据库
```
import pymysql

class MysqlManager(object):
    def __init__(self, host='127.0.0.1', db='Test', user='root', password='123456', port=3306, charset='utf8mb4'):
        # 初始化数据库
        self.__host = host
        self.__db = db
        self.__user = user
        self.__password = password
        self.__port = port
        self.__charset = charset
        self.__connect = None
        self.__cursor = None

    def _connect_db(self):
        # 连接数据库
        params = {
            'host': self.__host,
            'db': self.__db,
            'user': self.__user,
            'password': self.__password,
            'port': self.__port,
            'charset': self.__charset
        }
        self.__connect = pymysql.connect(**params)
        self.__cursor = self.__connect.cursor()

    def _close_db(self):
        # 关闭数据库
        self.__cursor.close()
        self.__connect.close()

    def _deal_values(self, value):
        if isinstance(value, str):
            # 如果是字符串则加上''
            value = ("'{value}'".format(value=value))
        elif isinstance(value, dict):
            # 如果是字典则变成key=value形式
            result = []
            for key, value in value.items():
                value = self._deal_values(value)
                res = "{key}={value}".format(key=key, value=value)
                result.append(res)
            return result
        else:
            value = (str(value))
        return value

    def insert(self, table, insert_data):
        # 插入数据：table为str类型、insert_data为包含dict的list；返回数据insert_num为int类型。
        self._connect_db()
        try:
            insert_num = 0
            for single_data in insert_data:
                key = ','.join(single_data.keys())
                values = map(self._deal_values, single_data.values())
                insert_data = ', '.join(values)
                sql = 'insert into {table}({key}) values ({val})'.format(table=table, key=key, val=insert_data)
                execute_num = self.__cursor.execute(sql)
                self.__connect.commit()
                insert_num += execute_num
            return insert_num
        except Exception as e:
            self.__connect.rollback()
            print(e)
        finally:
            self._close_db()

    def delete(self, table, condition):
        # 删除数据：table为str类型、condition为dict类型；返回数据delete_num为int类型。
        self._connect_db()
        try:
            # 处理删除的条件
            condition_list = self._deal_values(condition)
            condition_data = ' and '.join(condition_list)
            # 构建sql语句
            sql = 'delete from {table} where {condition}'.format(table=table, condition=condition_data)
            delete_num = self.__cursor.execute(sql)
            self.__connect.commit()
            return delete_num
        except Exception as e:
            self.__connect.rollback()
            print(e)
        finally:
            self._close_db()

    def update(self, table, update_data, condition=None):
        # 修改数据：table为str类型、update_data为dict类型、condition为dict类型；返回数据update_num为int类型。
        self._connect_db()
        try:
            # 处理传入的数据
            update_list = self._deal_values(update_data)
            update_data_list = ','.join(update_list)
            # 判断是否有条件
            if condition is not None:
                # 处理传入的条件
                condition_list = self._deal_values(condition)
                condition_data = ' and '.join(condition_list)
                sql = 'update {table} set {values} where {condition}'.format(table=table, values=update_data_list,
                                                                             condition=condition_data)
            else:
                sql = 'update {table} set {values}'.format(table=table, values=update_data_list)
            update_num = self.__cursor.execute(sql)
            self.__connect.commit()
            return update_num
        except Exception as e:
            self.__connect.rollback()
            print(e)
        finally:
            self._close_db()

    def select(self, table, show_list, condition=None, get_one=False):
        # 查询数据：table为str类型、show_list为list类型、condition为dict类型、get_one为Boolean类型；返回数据result为tuple类型。
        self._connect_db()
        try:
            # 处理显示的数据
            show_list = ','.join(show_list)
            sql = 'select {key} from {table}'.format(key=show_list, table=table)
            # 处理传入的条件
            if condition:
                condition_list = self._deal_values(condition)
                condition_data = 'and'.join(condition_list)
                sql = 'select {key} from {table} where {condition}'.format(key=show_list, table=table,
                                                                           condition=condition_data)
            self.__cursor.execute(sql)
            # 返回一条数据还是所有数据
            if get_one:
                result_data = self.__cursor.fetchone()
            else:
                result_data = self.__cursor.fetchall()
            return result_data
        except Exception as e:
            print(e)
        finally:
            self._close_db()
```