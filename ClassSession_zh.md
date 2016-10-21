# `类 tensorflow::Session`

一个Session的实例让调用者可以执行Tensorfow中的图
当一个Session的对象根据给定的目标创建之后，这个新的Session对象就被绑定到这个目标的特定资源。
Session就可以使用这些资源来执行GraphDef描述的“图”
当一个Session通过一个“图”扩展之后，调用者使用函数Run()接口执行计算，或者获取Tensors型输出数据。


例子:

```c++ tensorflow::GraphDef graph;
// 创建并加载一个“图”

// 这个例子使用一个当前运行环境的默认选项

tensorflow::SessionOptions options;
std::unique_ptr<tensorflow::Session>
session(tensorflow::NewSession(options));

// 根据graph创建一个Session
tensorflow::Status s = session->Create(graph);
if (!s.ok()) { ... }

// 执行“图”，并取得"output"的第一个输出数据
// 执行但不返回任何东西给"update_state"
std::vector<tensorflow::Tensor> outputs;
s = session->Run({}, {"output:0"}, {"update_state"}, &outputs);
if (!s.ok()) { ... }

// 重新“格式化“输出的float数据，可以对“格式化”之后的数据进行操作
//（tensor一般是多维数据可以通过flat函数，标准化成一维的）
auto output_tensor = outputs[0].flat<float>();
if (output_tensor(0) > 0.5) { ... }

// 关闭session并释放资源
session->Close();

```

一个Session可以并行的执行Run()，但是创建/扩展只能在单线程中执行。
只有一个线程可以调用Close()，在调用Cloase()之前所有的Run()都必须返回。
###Member Details

#### `tensorflow::Session::Session()` {#tensorflow_Session_Session}





#### `virtual tensorflow::Session::~Session()` {#virtual_tensorflow_Session_Session}





#### `virtual Status tensorflow::Session::Create(const GraphDef &graph)=0` {#virtual_Status_tensorflow_Session_Create}


创建一个Session使用的图。

如果当前Session已经被一个“图”创建就返回错误，如果Session使用与不同的“图”那么必须在使用之前调用Close()。
#### `virtual Status tensorflow::Session::Extend(const GraphDef &graph)=0` {#virtual_Status_tensorflow_Session_Extend}

向Session注册的“图”添加操作。

对“图”新操作的名字之前不能被注册过

#### `virtual Status tensorflow::Session::Run(const std::vector< std::pair< string, Tensor > > &inputs, const std::vector< string > &output_tensor_names, const std::vector< string > &target_node_names, std::vector< Tensor > *outputs)=0` {#virtual_Status_tensorflow_Session_Run}


执行一张图，inputs存储输入的数据。结束后将根据给定的输出节点的名字将数据填充到outputs中。

会执行到名为target_node_names的节点，但是不会返回对应的Tensor数据。

outputs中的tensor的顺序必须和output_tensor_names中的名字对应。

如果Run返回OK()，那么outputs的个数应等于output_tensor_names的个数。

如果Run()不返回OK那么outputs的状态为指定。

要求：输入和输出的tensor变量的名字要和函数creat()，传入的“图定义”GraphDef中的tensor节点对应。

要求：output_tensor_names和target_node_names至少一个不为空

要求：output_tensor_names不为空，那么outputs不能为空。

#### `virtual Status tensorflow::Session::Create(const RunOptions &run_options, const GraphDef &graph)` {#virtual_Status_tensorflow_Session_Create}


执行创建“图”的时候加入RunOptions选项，

注意：这个API仍然在实验中，有可能会改变。

#### `virtual Status tensorflow::Session::Extend(const RunOptions &run_options, const GraphDef &graph)` {#virtual_Status_tensorflow_Session_Extend}





#### `virtual Status tensorflow::Session::Close(const RunOptions &run_options)` {#virtual_Status_tensorflow_Session_Close}





#### `virtual Status tensorflow::Session::Run(const RunOptions &run_options, const std::vector< std::pair< string, Tensor > > &inputs, const std::vector< string > &output_tensor_names, const std::vector< string > &target_node_names, std::vector< Tensor > *outputs, RunMetadata *run_metadata)` {#virtual_Status_tensorflow_Session_Run}


和函数Run一样，但是允许调用者传入一个RunOption的选项。

同时可以通过参数RunMetadata取回非tensor类型的“元数据”。

run_metadata可能为空，如果为空就不会输出元数据。

注意：这个API仍然在实验中，有可能改变。

#### `virtual Status tensorflow::Session::PRunSetup(const std::vector< string > &input_names, const std::vector< string > &output_names, const std::vector< string > &target_nodes, string *handle)` {#virtual_Status_tensorflow_Session_PRunSetup}


设置一个“图”部分执行。

所有的填充和取回数据都通过input_names和output_names标定。

返回的handle用于执行一系列的部分填充和取回。

注意：这个API仍然在实验中，有可能改变。



#### `virtual Status tensorflow::Session::PRun(const string &handle, const std::vector< std::pair< string, Tensor > > &inputs, const std::vector< string > &output_names, std::vector< Tensor > *outputs)` {#virtual_Status_tensorflow_Session_PRun}

执行handle指定的“图”（不确定）

input用于填充输入数据，将output_names标注的节点数据输出到outputs中。

注意：这个API仍然在实验中，有可能改变。



#### `virtual Status tensorflow::Session::Close()=0` {#virtual_Status_tensorflow_Session_Close}


关闭一个Session

关闭session并释放tensorflow运行期间的资源（特别是SessionOptions::target指定创建的资源）。

