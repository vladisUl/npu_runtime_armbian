radxa-q6a-npu-runtime-working-kit

Target:
  Radxa Dragon Q6A / QCS6490 / DSP arch v68

Purpose:
  Working FastRPC + CDSP/NPU runtime kit for running QAIRT/QNN HTP on Armbian.

Verified working:
  Armbian kernel:
    6.18.2-current-qcs6490

  Docker image:
    radxazifeng278/qairt-npu-v68:v1.2

  QAIRT/QNN:
    QAIRT SDK 2.42.0.251225
    qnn-net-run v2.37.1
    ResNet50 context binary for PRODUCT_SOC=6490

Source:
  Runtime files extracted from Radxa OS rootfs.

Important finding:
  The Armbian cdsp.mbn/adsp.mbn firmware did not work with the Radxa OS DSP runtime.
  qnn-net-run failed at FastRPC session creation:
    FASTRPC_IOCTL_INIT_CREATE
    qnn_open failed, 0x80000600

  Replacing Armbian firmware with Radxa OS firmware fixed the issue.

Working firmware checksums:
  cdsp.mbn:
    a9645c9418bc8a456fab8e379c9e30b1c6135a32fc2b91e99e77cbded93a1d56

  adsp.mbn:
    01fea79407e512a644fa33a7882af9958b043cfe5389aba81ca91fb74267a5f3

Contains:
  /usr/bin/*dsprpcd daemons
  /usr/lib/libcdsprpc.so*
  /usr/lib/dsp/adsp/*
  /usr/lib/dsp/cdsp/*
  /usr/lib/udev/rules.d/99-fastrpc.rules
  /lib/firmware/qcom/qcs6490/radxa/dragon-q6a/adsp.mbn
  /lib/firmware/qcom/qcs6490/radxa/dragon-q6a/cdsp.mbn
  /lib/firmware/qcom/qcs6490/radxa/dragon-q6a/adspr.jsn
  /lib/firmware/qcom/qcs6490/radxa/dragon-q6a/adspua.jsn
  /lib/firmware/qcom/qcs6490/radxa/dragon-q6a/cdspr.jsn

Does not contain:
  fastrpc-test / fastrpc_test

Install note:
  Reboot is required after installing firmware files.

Proof of work:
  qnn-net-run executed resnet50_aimet_quantized_6490.bin successfully.
  Log contained:
    Using fastrpc for execution
    Graph resnet50_aimet execution finished with result 0
    Finished Executing Graphs
