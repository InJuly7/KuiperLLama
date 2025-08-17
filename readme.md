## 硬件平台
Jetson Orin Nx

## 第三方依赖
```bash
git@github.com:google/sentencepiece.git               v0.2.0
git@github.com:google/glog.git                        v0.7.1
https://gitlab.com/conradsnicta/armadillo-code.git    14.0.1
git@github.com:google/re2.git                         2022-04-01
git@github.com:abseil/abseil-cpp.git                  20240722.0
git@github.com:google/googletest.git                  v1.15.0
git@github.com:nlohmann/json.git                      v3.11.3

song@Orin-Nx:~/program/KuiperLLama/build/demo$ ldd qwen_infer 
        libglog.so.2 => /usr/local/lib/libglog.so.2 (0x0000ffffbd470000)
        libsentencepiece.so.0 => /usr/local/lib/libsentencepiece.so.0 (0x0000ffffbcf10000)
        libarmadillo.so.14 => /usr/lib/aarch64-linux-gnu/libarmadillo.so.14 (0x0000ffffbce00000)
        libre2.so.9 => /usr/local/lib/libre2.so.9 (0x0000ffffbcd70000)
        libgflags.so.2.2 => /usr/lib/aarch64-linux-gnu/libgflags.so.2.2 (0x0000ffffbcc90000)
        libunwind.so.8 => /usr/lib/aarch64-linux-gnu/libunwind.so.8 (0x0000ffffbcc50000)
```

# Qwen2.5 推理

- 以 Qwen2.5-0.5B 为例，huggingface 上下载模型：
```shell
export HF_ENDPOINT=https://hf-mirror.com
pip3 install huggingface-cli
huggingface-cli download --resume-download Qwen/Qwen2.5-0.5B --local-dir Qwen/Qwen2.5-0.5B --local-dir-use-symlinks False
```

- 导出模型：
```shell
python3 tools/export_qwen2.py Qwen2.5-0.5B.bin --hf=Qwen/Qwen2.5-0.5B
```

- 编译：
```shell
mkdir build && cd build
cmake -DQWEN2_SUPPORT=ON ..  && make -j16
```
- 运行：
```shell
./build/demo/qwen_infer Qwen2.5-0.5B.bin Qwen/Qwen2.5-0.5B/tokenizer.json
# 和 huggingface 推理的结果进行对比
python3 hf_infer/qwen2_infer.py
```