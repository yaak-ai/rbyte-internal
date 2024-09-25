<p align="center">
 <a href="https://www.yaak.ai/open-source/dev-tools">
  <img alt="banner" src="https://github.com/user-attachments/assets/707ab3ae-73d5-459f-82c5-888323673adb">
 </a>
</p>

<p align="center">
 <img src="https://github.com/yaak-ai/rbyte/actions/workflows/ci.yaml/badge.svg">
 <img src="https://img.shields.io/github/license/yaak-ai/rbyte.svg?color=green"></a>
</p>


`rbyte` provides a [PyTorch](https://pytorch.org) [`Dataset`](https://pytorch.org/tutorials/beginner/basics/data_tutorial.html) with [`tensorclass`](https://pytorch.org/tensordict/main/reference/tensorclass.html) samples built from multimodal data

## Installation

```bash
uv add https://github.com/yaak-ai/rbyte/releases/latest/download/rbyte-X.Y.Z-py3-none-any.whl [--extra mcap] [--extra jpeg] [--extra visualize]
```

## Examples

See [examples/config_templates](examples/config_templates) ([`ytt`](https://carvel.dev/ytt/) templates) and [justfile](justfile) for usage examples.

<details>
<summary><a href=https://nuscenes.org> nuScenes </a> x <a href=https://mcap.dev> mcap </a></summary>

1. Setup a new project with [`uv`](https://docs.astral.sh/uv/)
```shell
uv init nuscenes_mcap
cd nuscenes_mcap

uv add hydra-core omegaconf
uv add https://github.com/yaak-ai/rbyte/releases/latest/download/rbyte-0.2.0-py3-none-any.whl --extra mcap --extra jpeg --extra visualize

mkdir data
```

2. Follow the guide at [foxglove/nuscenes2mcap](https://github.com/foxglove/nuscenes2mcap) and move the resulting `.mcap` files under `data/`. In this example we're using a subset of topics from `NuScenes-v1.0-mini-scene-0103.mcap`:
```shell
mcap info data/NuScenes-v1.0-mini-scene-0103.mcap
library:   nuscenes2mcap
profile:
messages:  34764
duration:  19.443428s
start:     2018-08-01T21:26:43.504799+02:00 (1533151603.504799000)
end:       2018-08-01T21:27:02.948227+02:00 (1533151622.948227000)
compression:
        lz4: [629/629 chunks] [753.36 MiB/481.51 MiB (36.09%)] [24.76 MiB/sec]
channels:
        (1)  /imu                                     1933 msgs (99.42 Hz)     : IMU [jsonschema]
        (2)  /odom                                     968 msgs (49.79 Hz)     : Pose [jsonschema]
        (3)  /map                                        1 msgs                : foxglove.Grid [protobuf]
        (4)  /semantic_map                               1 msgs                : foxglove.SceneUpdate [protobuf]
        (5)  /tf                                      3103 msgs (159.59 Hz)    : foxglove.FrameTransform [protobuf]
        (6)  /drivable_area                             40 msgs (2.06 Hz)      : foxglove.Grid [protobuf]
        (7)  /RADAR_FRONT                              266 msgs (13.68 Hz)     : foxglove.PointCloud [protobuf]
        (8)  /RADAR_FRONT_LEFT                         258 msgs (13.27 Hz)     : foxglove.PointCloud [protobuf]
        (9)  /RADAR_FRONT_RIGHT                        259 msgs (13.32 Hz)     : foxglove.PointCloud [protobuf]
        (10) /RADAR_BACK_LEFT                          252 msgs (12.96 Hz)     : foxglove.PointCloud [protobuf]
        (11) /RADAR_BACK_RIGHT                         255 msgs (13.11 Hz)     : foxglove.PointCloud [protobuf]
        (12) /LIDAR_TOP                                389 msgs (20.01 Hz)     : foxglove.PointCloud [protobuf]
        (13) /CAM_FRONT/image_rect_compressed          229 msgs (11.78 Hz)     : foxglove.CompressedImage [protobuf]
        (14) /CAM_FRONT/camera_info                    229 msgs (11.78 Hz)     : foxglove.CameraCalibration [protobuf]
        (15) /CAM_FRONT/lidar                          229 msgs (11.78 Hz)     : foxglove.ImageAnnotations [protobuf]
        (16) /CAM_FRONT/annotations                     40 msgs (2.06 Hz)      : foxglove.ImageAnnotations [protobuf]
        (17) /CAM_FRONT_RIGHT/image_rect_compressed    233 msgs (11.98 Hz)     : foxglove.CompressedImage [protobuf]
        (18) /CAM_FRONT_RIGHT/camera_info              233 msgs (11.98 Hz)     : foxglove.CameraCalibration [protobuf]
        (19) /CAM_FRONT_RIGHT/lidar                    233 msgs (11.98 Hz)     : foxglove.ImageAnnotations [protobuf]
        (20) /CAM_FRONT_RIGHT/annotations               40 msgs (2.06 Hz)      : foxglove.ImageAnnotations [protobuf]
        (21) /CAM_BACK_RIGHT/image_rect_compressed     234 msgs (12.03 Hz)     : foxglove.CompressedImage [protobuf]
        (22) /CAM_BACK_RIGHT/camera_info               234 msgs (12.03 Hz)     : foxglove.CameraCalibration [protobuf]
        (23) /CAM_BACK_RIGHT/lidar                     234 msgs (12.03 Hz)     : foxglove.ImageAnnotations [protobuf]
        (24) /CAM_BACK_RIGHT/annotations                40 msgs (2.06 Hz)      : foxglove.ImageAnnotations [protobuf]
        (25) /CAM_BACK/image_rect_compressed           229 msgs (11.78 Hz)     : foxglove.CompressedImage [protobuf]
        (26) /CAM_BACK/camera_info                     229 msgs (11.78 Hz)     : foxglove.CameraCalibration [protobuf]
        (27) /CAM_BACK/lidar                           229 msgs (11.78 Hz)     : foxglove.ImageAnnotations [protobuf]
        (28) /CAM_BACK/annotations                      40 msgs (2.06 Hz)      : foxglove.ImageAnnotations [protobuf]
        (29) /CAM_BACK_LEFT/image_rect_compressed      228 msgs (11.73 Hz)     : foxglove.CompressedImage [protobuf]
        (30) /CAM_BACK_LEFT/camera_info                228 msgs (11.73 Hz)     : foxglove.CameraCalibration [protobuf]
        (31) /CAM_BACK_LEFT/lidar                      228 msgs (11.73 Hz)     : foxglove.ImageAnnotations [protobuf]
        (32) /CAM_BACK_LEFT/annotations                 40 msgs (2.06 Hz)      : foxglove.ImageAnnotations [protobuf]
        (33) /CAM_FRONT_LEFT/image_rect_compressed     231 msgs (11.88 Hz)     : foxglove.CompressedImage [protobuf]
        (34) /CAM_FRONT_LEFT/camera_info               231 msgs (11.88 Hz)     : foxglove.CameraCalibration [protobuf]
        (35) /CAM_FRONT_LEFT/lidar                     231 msgs (11.88 Hz)     : foxglove.ImageAnnotations [protobuf]
        (36) /CAM_FRONT_LEFT/annotations                40 msgs (2.06 Hz)      : foxglove.ImageAnnotations [protobuf]
        (37) /pose                                      40 msgs (2.06 Hz)      : foxglove.PoseInFrame [protobuf]
        (38) /gps                                       40 msgs (2.06 Hz)      : foxglove.LocationFix [protobuf]
        (39) /markers/annotations                       40 msgs (2.06 Hz)      : foxglove.SceneUpdate [protobuf]
        (40) /markers/car                               40 msgs (2.06 Hz)      : foxglove.SceneUpdate [protobuf]
        (41) /diagnostics                            22487 msgs (1156.53 Hz)   : diagnostic_msgs/DiagnosticArray [ros1msg]
attachments: 0
metadata: 1
```

3. Create a `config.yaml` with the following:
```yaml
---
dataloader:
  _target_: torch.utils.data.DataLoader
  dataset: ${dataset}
  batch_size: 32
  collate_fn:
    _target_: rbyte.utils.dataloader.collate_identity
    _partial_: true

dataset:
  _target_: rbyte.Dataset
  _convert_: all
  _recursive_: false
  inputs:
    NuScenes-v1.0-mini-scene-0103:
      frame:
        CAM_FRONT:
          index_column: /CAM_FRONT/image_rect_compressed/frame_idx
          reader:
            _target_: rbyte.io.frame.mcap.McapFrameReader
            path: data/NuScenes-v1.0-mini-scene-0103.mcap
            topic: /CAM_FRONT/image_rect_compressed
            message_decoder_factory: ${message_decoder_factory}
            frame_decoder: ${jpeg_decoder}

        CAM_FRONT_LEFT:
          index_column: /CAM_FRONT_LEFT/image_rect_compressed/frame_idx
          reader:
            _target_: rbyte.io.frame.mcap.McapFrameReader
            path: data/NuScenes-v1.0-mini-scene-0103.mcap
            topic: /CAM_FRONT_LEFT/image_rect_compressed
            message_decoder_factory: ${message_decoder_factory}
            frame_decoder: ${jpeg_decoder}

      table:
        path: data/NuScenes-v1.0-mini-scene-0103.mcap
        builder:
          _target_: rbyte.io.table.TableBuilder
          reader:
            _target_: rbyte.io.table.mcap.McapProtobufTableReader
            _recursive_: false
            _convert_: all
            fields:
              /CAM_FRONT/image_rect_compressed:
                log_time:
                  _target_: polars.Datetime
                  time_unit: ns

              /CAM_FRONT_LEFT/image_rect_compressed:
                log_time:
                  _target_: polars.Datetime
                  time_unit: ns

              /gps:
                log_time:
                  _target_: polars.Datetime
                  time_unit: ns

                latitude: polars.Float64
                longitude: polars.Float64

          merger:
            _target_: rbyte.io.table.TableMerger
            separator: /
            merge:
              /CAM_FRONT/image_rect_compressed:
                log_time:
                  method: ref

              /CAM_FRONT_LEFT/image_rect_compressed:
                log_time:
                  method: ref
                frame_idx:
                  method: asof
                  tolerance: 100ms
                  strategy: nearest

              /gps:
                log_time:
                  method: ref
                latitude:
                  method: asof
                  tolerance: 1000ms
                  strategy: nearest
                longitude:
                  method: asof
                  tolerance: 1000ms
                  strategy: nearest

  sample_builder:
    _target_: rbyte.sample.builder.GreedySampleTableBuilder
    index_column: /CAM_FRONT/image_rect_compressed/frame_idx
    length: 1
    stride: 1
    min_step: 1

jpeg_decoder:
  _target_: simplejpeg.decode_jpeg
  _partial_: true
  colorspace: rgb

message_decoder_factory:
  _target_: mcap_protobuf.decoder.DecoderFactory
```

3. Build a dataloader and inspect a batch:
```python
from omegaconf import OmegaConf
from hydra.utils import instantiate


config = OmegaConf.load("config.yaml")
dataloader = instantiate(config.dataloader)
batch = next(iter(dataloader))
print(batch)
```

```python
Batch(
    frame=TensorDict(
        fields={
            CAM_BACK: Tensor(shape=torch.Size([32, 1, 900, 1600, 3]), device=cpu, dtype=torch.uint8, is_shared=False),
            CAM_FRONT: Tensor(shape=torch.Size([32, 1, 900, 1600, 3]), device=cpu, dtype=torch.uint8, is_shared=False)},
        batch_size=torch.Size([32]),
        device=None,
        is_shared=False),
    meta=BatchMeta(
        input_id=NonTensorData(data=['NuScenes-v1.0-mini  ...  .0-mini-scene-0103'], batch_size=torch.Size([32]), device=None),
        sample_idx=Tensor(shape=torch.Size([32]), device=cpu, dtype=torch.int64, is_shared=False),
        batch_size=torch.Size([32]),
        device=None,
        is_shared=False),
    table=TensorDict(
        fields={
            /CAM_BACK/image_rect_compressed/frame_idx: Tensor(shape=torch.Size([32, 1]), device=cpu, dtype=torch.int64, is_shared=False),
            /CAM_BACK/image_rect_compressed/log_time: Tensor(shape=torch.Size([32, 1]), device=cpu, dtype=torch.int64, is_shared=False),
            /CAM_FRONT/image_rect_compressed/frame_idx: Tensor(shape=torch.Size([32, 1]), device=cpu, dtype=torch.int64, is_shared=False),
            /CAM_FRONT/image_rect_compressed/log_time: Tensor(shape=torch.Size([32, 1]), device=cpu, dtype=torch.int64, is_shared=False),
            /gps/latitude: Tensor(shape=torch.Size([32, 1]), device=cpu, dtype=torch.float64, is_shared=False),
            /gps/log_time: Tensor(shape=torch.Size([32, 1]), device=cpu, dtype=torch.int64, is_shared=False),
            /gps/longitude: Tensor(shape=torch.Size([32, 1]), device=cpu, dtype=torch.float64, is_shared=False)},
        batch_size=torch.Size([32]),
        device=None,
        is_shared=False),
    batch_size=torch.Size([32]),
    device=None,
    is_shared=False)

```

<details>
<summary>(optional) <a href=https://rerun.io.> rerun </a> visualization </a></summary>

4. Add a `logger` to `config.yaml`:
```yaml
# ...

logger:
  _target_: rbyte.viz.loggers.RerunLogger
  schema:
    frame:
      CAM_FRONT: rerun.components.ImageBufferBatch
      CAM_FRONT_LEFT: rerun.components.ImageBufferBatch

    table:
      /CAM_FRONT/image_rect_compressed/log_time: rerun.TimeNanosColumn
      /CAM_FRONT_LEFT/image_rect_compressed/frame_idx: rerun.TimeSequenceColumn
      /gps/log_time: rerun.TimeNanosColumn
      /gps/latitude: rerun.components.ScalarBatch
      /gps/longitude: rerun.components.ScalarBatch
```

5. Visualize the dataset:
```python
from omegaconf import OmegaConf
from hydra.utils import instantiate


config = OmegaConf.load("config.yaml")
dataloader = instantiate(config.dataloader)
logger = instantiate(config.logger)

for batch_idx, batch in enumerate(dataloader):
    logger.log(batch_idx, batch)
```
<img width="1510" alt="image" src="https://github.com/user-attachments/assets/f00d6b66-9e8b-4dce-9f1e-6af281141f84">

</details>

</details>

 

## Development

1. Install required tools:
- [`uv`](https://github.com/astral-sh/uv)
- [`just`](https://github.com/casey/just)

2. Clone:
```bash
git clone https://github.com/yaak-ai/rbyte
```

3. Setup:
```shell
just setup
```
