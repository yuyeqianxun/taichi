# Tai Chi

HG 可视化前端项目

## 开发

项目使用`ant-design-pro`进行搭建，技术栈如下：  
`react + typescript + dva + umi + CSS module`

参与开发：

```shell script
# 克隆代码
git clone git@github.com:HelloGitHub-Team/taichi.git
# 进入项目目录
cd taichi
# 安装依赖
yarn install
# 启动项目
yarn start
```

> **Tips :**
>
> - 启动时[去掉了`umi ui`可视化编程辅助工具](https://github.com/ant-design/ant-design-pro/issues/5411)
> - 开启了`DllPlugin`打包优化，如果启动[发现问题](https://github.com/ant-design/ant-design-pro/issues/4665)可以关闭

## 接口

### 响应约定

> **Tips**:服务端字段说明会在响应中以`#`出现

响应状态通过 `HTTP status code` 反映，规则如下：

- 200：成功
- 400：参数错误
- 401：未登录
- 403：禁止访问
- 404：未找到
- 500：服务器异常

成功响应模板：

```text
{
  "message": "OK", # 成功一般为 OK
  "payload": [] # 请求返回的数据
}
```

### 前端发起请求

#### 在`services`中定义请求的相关配置项和参数类型：

```typescript
export interface TestParams {
  userName: string;
  password: string;
  mobile: string;
  captcha: string;
}
export const fetchTest: AxiosRequestConfig = { url: '/api/test/success', method: 'post' };
```

#### 组件中使用：

```typescript
const ExampleTable = () => {
  const [dataSource, setDataSource] = useState([]);
  const [loading, setLoading] = useState(false);
  const params: TestParams = {
    userName: 'userName',
    password: 'password',
    mobile: 'mobile',
    captcha: 'captcha',
  };
  useEffect(() => {
    setLoading(true);
    request({ ...fetchTest, params }).then(
      response => {
        setLoading(false);
        setDataSource(response.payload);
      },
      error => {
        setLoading(false);
        console.log('error', error);
      },
    );
  }, []);
  return (
    <Card>
      <Table loading={loading} dataSource={dataSource} columns={columns} />
    </Card>
  );
};
```

#### 通过自定义`Hooks`使用：

```typescript
const ExampleTable = () => {
  const params: TestParams = {
    userName: 'userName',
    password: 'password',
    mobile: 'mobile',
    captcha: 'captcha',
  };
  const { response, loading, fetch } = useRequest({ ...fetchTest, params });
  const dataSource = response ? response.payload : [];
  useEffect(() => {
    fetch({ params }).then();
  }, []);
  return (
    <Card>
      <Table loading={loading} dataSource={dataSource} columns={columns} />
    </Card>
  );
};
```
