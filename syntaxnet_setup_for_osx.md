## Syntaxnet setup for OSx

I’ve setup tensorflow-syntaxnet in a virtual env named ‘tf’ in OsX. Please find the below steps that helped me in getting the build successful in OsX:

1. Setup virtual environment named ‘tf’.
  ```markdown
  virtualenv -p /usr/local/bin/python tf
  source tf/bin/activate
  ```
2. Install **six** - tensor flow dependency: 
```markdown
sudo easy_install --upgrade six
```
3. Install tensor flow:
```markdown
sudo pip install --upgrade tensorflow
```
4. Test tensor flow installation with helloworld in python terminal:
```markdown
python
import tensorflow as tf
hello = tf.constant('HelloWorld, Tensorflow!!')
sess = tf.session()
print(sess.run(hello))
```
5. Install SyntaxNet Dependencies:
 ```markdown
 brew install swig
 pip freeze | grep protobuf
 sudo pip install -U protobuf==3.2.0
 pip install asciitree
 pip install numpy
 pip install autograd==1.1.13
 Install bazel 0.5.4
   - Download bazel-0.5.4-without-jdk-installer-darwin-x86_64.sh from  https://github.com/bazelbuild/bazel/releases 
   - chmod +x ~/Downloads/bazel-0.5.4-without-jdk-installer-darwin-x86_64.sh
   - sh ~/Downloads/bazel-0.5.4-without-jdk-installer-darwin-x86_64.sh
 pip install mock
 brew install graphviz
 pip install pygraphviz
 ```
6. Download syntaxnet:
```markdown
cd <LOCATION_TO_WHERE_SYNTAXNET_REPO_WILL_BE_COPIED>
git clone --recursive https://github.com/tensorflow/models.git
```
7. Configure:
```markdown
cd <LOCATION_TO_WHERE_SYNTAXNET_REPO_IS_COPIED>/models/research/syntaxnet/tensorflow
./configure
```
8. Build Syntaxnet: 
 ```markdown
 cd <LOCATION_TO_WHERE_SYNTAXNET_REPO_IS_COPIED>
 run “bazel test --linkopt=-headerpad_max_install_names dragnn/... syntaxnet/... util/utf8/…”
 ```
  All the tests should pass with the above command and it will create bazel-bin directory in  `<LOCATION_TO_WHERE_SYNTAXNET_REPO_IS_COPIED>/models/research/syntaxnet/bazel-bin`. This is the bin folder using which we'll package syntaxnet as module and install it. Before that, let's test the installation.
9. Test Syntaxnet: 
```markdown
cd <LOCATION_TO_WHERE_SYNTAXNET_REPO_IS_COPIED>/models/research/syntaxnet
echo 'Bob brought the pizza to Alice.' | syntaxnet/demo.sh
```
10. Output:


![image](https://user-images.githubusercontent.com/22542670/38160793-93ae9d1c-34e0-11e8-813d-56298256858d.png)

11. Bundle syntaxnet package in `.whl packaging` format.
```markdown
mkdir /tmp/syntaxnet_pkg
cd <LOCATION_TO_WHERE_SYNTAXNET_REPO_IS_COPIED>/models/research/syntaxnet
bazel-bin/dragnn/tools/build_pip_package --output-dir=/tmp/syntaxnet_pkg
**Output:** Wrote /tmp/syntaxnet_pkg/syntaxnet-0.2-cp27-cp27m-macosx_10_6_intel.whl
```
12. Install syntaxnet module:
```markdown
sudo pip install /tmp/syntaxnet_pkg/syntaxnet-0.2-cp27-cp27m-macosx_10_6_intel.whl
```
### Output: Successfully installed syntaxnet-0.2
## Verification
###### Open up python shell and check that the following imports work fine
```markdown
python
from syntaxnet import sentence_pb2
from syntaxnet import graph_builder
from syntaxnet import structured_graph_builder
from syntaxnet.ops import load_parser_ops
from syntaxnet.ops import gen_parser_ops
from syntaxnet import task_spec_pb2
```

## Appendix - Hammering Syntaxnet to behave 
There were a bunch of errors I faced before I could reach this final point of successful installation with imports working in python shell. I've listed the issues that i faced [here](https://github.com/spoddutur/syntaxnet/blob/master/hammering_syntaxnet_to_behave.md) and how i went about fixing them here. Hope that saves some time for you :)

