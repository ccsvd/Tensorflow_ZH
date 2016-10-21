# TensorFlow C++"回话"相关的API文档

TensorFlow的0.5版本中公开C++代码只包含执行“graph”的API。以下是从C++代码中控制执行“图”。

1. 使用[Python API](../../api_docs/python/) 构建一个计算“图”。
1. 使用函数[tf.train.write_graph()](../../api_docs/python/train.md#write_graph)将一个“图”写入到文件。
1. 通过一下的方式使用C++的回话API加载一个“图”。

  ```c++
  // 从磁盘读取一个“图”的定义，接着创建一个回话对象。然后就可以执行
  Status LoadGraph(string graph_file_name, Session** session) {
    GraphDef graph_def;
    TF_RETURN_IF_ERROR(
        ReadBinaryProto(Env::Default(), graph_file_name, &graph_def));
    TF_RETURN_IF_ERROR(NewSession(SessionOptions(), session));
    TF_RETURN_IF_ERROR((*session)->Create(graph_def));
    return Status::OK();
  }
```

1. 调用函数`session->Run()`来运行一个“图”。

# 相关的类
## Env

* [tensorflow::Env](ClassEnv.md)
* [tensorflow::RandomAccessFile](ClassRandomAccessFile.md)
* [tensorflow::WritableFile](ClassWritableFile.md)
* [tensorflow::EnvWrapper](ClassEnvWrapper.md)

## Session

* [tensorflow::Session](ClassSession.md)
* [tensorflow::SessionOptions](StructSessionOptions.md)

## Status

* [tensorflow::Status](ClassStatus.md)
* [tensorflow::Status::State](StructState.md)

## Tensor

* [tensorflow::Tensor](ClassTensor.md)
* [tensorflow::TensorShape](ClassTensorShape.md)
* [tensorflow::TensorShapeDim](StructTensorShapeDim.md)
* [tensorflow::TensorShapeUtils](ClassTensorShapeUtils.md)
* [tensorflow::PartialTensorShape](ClassPartialTensorShape.md)
* [tensorflow::PartialTensorShapeUtils](ClassPartialTensorShapeUtils.md)

## Thread

* [tensorflow::Thread](ClassThread.md)
* [tensorflow::ThreadOptions](StructThreadOptions.md)