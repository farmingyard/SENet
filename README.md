# SENet
Squeeze-and-Excitation Networks, a new architecture building block proposed by WMW team at the ILSVRC2017 challenges(http://image-net.org/challenges/LSVRC/2017/results#team)

<div align=center>
<img src="http://pics.haik8.com/nqvaoZtoY6GlxJuvYKfWmZlkxaJhpc-bna-SoKCfYq/6vjJ9y0mykrX6qnW6sbbGbiquxgqifzq-saaqBlYmBb42f3JbEopedmpaDh6yi2XN3pdykZabUqauAaWCebYunqK_Pk5JthIGGyoN6Y5jXqKmMlGWbmZlum2Rjb6mwkp2jq6Ohz6A.jpg">
</div>

<div align=center>
Figure 1: Inception and resnet with SE module.
</div> 

For more information, please refer to this article(http://www.haik8.com/p/jxz4p.html)


## SE module with caffe

You can add followed module to any net structure.
```
...
layer {
  ...
  top: "xxx"
  ...
}
layer {
  name: "se2_1/pool"
  type: "Pooling"
  bottom: "xxx"
  top: "se2_1/pool"
  pooling_param {
    pool: AVE
    global_pooling: true
  }
}
layer {
  name: "se2_1/ip1"
  type: "InnerProduct"
  bottom: "se2_1/pool"
  top: "se2_1/ip1"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  inner_product_param {
    num_output: 2
    weight_filler {
      type: "msra"
    }
    bias_filler {
      type: "constant"
      value: 0
    }
  }
}
layer {
  name: "relu_se2_1/ip1"
  type: "ReLU"
  bottom: "se2_1/ip1"
  top: "se2_1/ip1"
}
layer {
  name: "se2_1/ip2"
  type: "InnerProduct"
  bottom: "se2_1/ip1"
  top: "se2_1/ip2"
  param {
    lr_mult: 1
    decay_mult: 1
  }
  param {
    lr_mult: 2
    decay_mult: 0
  }
  inner_product_param {
    num_output: 32
    weight_filler {
      type: "msra"
    }
    bias_filler {
      type: "constant"
      value: 0
    }
  }
}
layer {
  name: "sigmoid_se2_1/ip2"
  type: "Sigmoid"
  bottom: "se2_1/ip2"
  top: "se2_1/ip2"
}
layer {
  name: "se2_1/scale"
  type: "Scale"
  bottom: "conv2_1/sep"
  bottom: "se2_1/ip2"
  top: "se2_1/scale"
  scale_param {
    axis: 0
  }
}
layer {
  ...
  bottom: "se2_1/scale"
  ...
}
...
```
