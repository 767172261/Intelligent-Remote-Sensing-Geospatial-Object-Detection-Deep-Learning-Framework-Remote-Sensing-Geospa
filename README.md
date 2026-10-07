**基于YOLO+DeepSeek的遥感地理空间目标检测系统**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/b5b98c5683b7428f80bb344cb1ea8d9b.png)

**使用NWPU遥感数据集
系统功能：
1.图像检测
2.批量检测
3.视频检测
4.图片、视频识别记录
5.用户管理
6.个人中心**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/bfd4ac11aaa24e19889d59292af39f16.png)

**系统特点：
-支持飞机、篮球场、足球场、船、网球场地等多种目标检测
-实时预测分析，准确率高
-支持图片、视频两种检测方式
-提供详细检测结果和分析报告
-界面简洁美观，操作简单
-支持AI智能建议**
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/122b1e4d94314f4fbb89b57ea7e3bb2a.png)

**系统技术栈：
前端：Vue3、Element-Plus
后端：SpringBoot、Flask
深度学习：YOLOv11、Pytorch
数据库：MySQL
视频处理：FFmpeg
适用场景：
遥感数据分析
地理空间目标检测**

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/6c492652e23d441b850c26b33914c435.png)
1
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/487894b87cd84ca6b7854b0c6165e8d9.png)
1
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/f9e513a9ee734048a90671c5b07e1d70.png)
1
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/e10e98ad28ce41ee9a287b9d7e120e23.png)
1
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/0fdddd2c96e742b3a630fe9e9af8390f.png)
1
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/2e18a770a7b54971a7d7923146021f61.png)
 **基于 YOLO + DeepSeek 的遥感地理空间目标检测系统** 的完整技术架构、功能解析与 **详细代码实现（含前后端）**，支持：

✅ 多类目标识别：飞机、篮球场、足球场、船、网球场等  
✅ 图片/视频/批量检测  
✅ AI智能分析报告生成（DeepSeek / Qwen）  
✅ 实时预测 + 可视化统计  
✅ 用户管理 + 识别记录查询  
✅ 前后端分离（Vue3 + Spring Boot）  

---

## ✅ 一、系统概览

| 项目 | 内容 |
|------|------|
| **系统名称** | 遥感地理空间目标检测系统（Remote Sensing Object Detection System） |
| **核心算法** | YOLOv11 + DeepSeek 大模型 |
| **检测类别** | 飞机、篮球场、足球场、船、网球场、桥梁、道路等（共10类） |
| **数据集** | NWPU-RESISC45 遥感数据集（约45类，精选常用类） |
| **准确率** | >92%（在测试集上） |
| **部署方式** | Web 端 + 后端服务 + MySQL 数据库 |
| **技术栈** |  
| - 前端：Vue3 + TypeScript + Element Plus + ECharts + Axios  
| - 后端：SpringBoot + MyBatis-Plus + Flask（AI推理）  
| - AI 模型：YOLOv11 + ONNX 导出 + DeepSeek API  
| - 视频处理：FFmpeg  
| - 数据库：MySQL  
| - 部署：Docker + Nginx |

---

## ✅ 二、系统架构图

```
┌────────────────────┐     ┌────────────────────┐
│    前端 (Vue3)       │◄───►│   后端 (SpringBoot)  │
│  (Web界面 + ECharts) │     │  (API + 用户管理)    │
└────────────────────┘     └────────────────────┘
           ↑                         ↑
           │                         │
           │                         │
           ▼                         ▼
┌────────────────────┐     ┌────────────────────┐
│   AI 推理 (Flask)    │◄───►│   大模型 (DeepSeek/Qwen) │
│  (YOLOv11 + OpenCV)  │     │  (API 调用)          │
└────────────────────┘     └────────────────────┘
           ↑                         ↑
           │                         │
           │                         │
           ▼                         ▼
┌────────────────────┐     ┌────────────────────┐
│   MySQL 数据库       │     │   文件存储 (MinIO)   │
└────────────────────┘     └────────────────────┘
```

---

## ✅ 三、10 类遥感目标分类表

```json
[
  "airplane",
  "basketball_court",
  "football_field",
  "ship",
  "tennis_court",
  "bridge",
  "road",
  "building",
  "railway",
  "vehicle"
]
```

> 💡 **数据集建议**：使用 `NWPU-RESISC45` 中的高分辨率遥感图像，标注清晰边界框。

---

## ✅ 四、前端代码（Vue3 + TypeScript）

### 1. `src/router/index.ts`

```ts
import { createRouter, createWebHistory } from 'vue-router'
import Home from '@/views/Home.vue'
import Login from '@/views/Login.vue'
import ImageDetect from '@/views/ImageDetect.vue'
import BatchDetect from '@/views/BatchDetect.vue'
import VideoDetect from '@/views/VideoDetect.vue'
import ImageRecord from '@/views/ImageRecord.vue'
import VideoRecord from '@/views/VideoRecord.vue'
import UserManage from '@/views/UserManage.vue'
import Profile from '@/views/Profile.vue'

const routes = [
  { path: '/login', component: Login },
  { path: '/', component: Home, children: [
    { path: '', redirect: '/image' },
    { path: 'image', component: ImageDetect },
    { path: 'batch', component: BatchDetect },
    { path: 'video', component: VideoDetect },
    { path: 'image-record', component: ImageRecord },
    { path: 'video-record', component: VideoRecord },
    { path: 'user', component: UserManage },
    { path: 'profile', component: Profile }
  ]}
]

const router = createRouter({
  history: createWebHistory(),
  routes
})

export default router
```

---

### 2. `src/views/ImageDetect.vue` —— 图片检测页面

```vue
<template>
  <div class="detect-container">
    <h2>图像检测</h2>

    <div class="upload-area">
      <input type="file" @change="handleImageUpload" accept="image/*" />
      <button @click="startDetect">开始预测</button>
      <button @click="exportPDF">PDF导出</button>
    </div>

    <div class="result-area">
      <img v-if="originalImage" :src="originalImage" alt="原图" />
      <img v-if="resultImage" :src="resultImage" alt="检测结果" />

      <div class="details">
        <p><strong>检测到目标：</strong> {{ detectedObjects }}</p>
        <p><strong>总耗时：</strong> {{ inferenceTime }}s</p>
      </div>
    </div>

    <div class="ai-advice">
      <h3>AI建议</h3>
      <pre>{{ aiAdvice }}</pre>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import axios from 'axios'

const originalImage = ref<string>()
const resultImage = ref<string>()
const detectedObjects = ref<string[]>([])
const inferenceTime = ref<number>(0)
const aiAdvice = ref<string>('')

const handleImageUpload = (e: Event) => {
  const file = (e.target as HTMLInputElement).files?.[0]
  if (file) {
    const reader = new FileReader()
    reader.onload = (event) => {
      originalImage.value = event.target?.result as string
    }
    reader.readAsDataURL(file)
  }
}

const startDetect = async () => {
  const formData = new FormData()
  formData.append('image', (document.querySelector('input[type=file]') as HTMLInputElement).files![0])

  const res = await axios.post('/api/detect/image', formData, {
    headers: { 'Content-Type': 'multipart/form-data' }
  })

  resultImage.value = `data:image/jpeg;base64,${res.data.image_base64}`
  detectedObjects.value = res.data.detected_objects
  inferenceTime.value = parseFloat(res.data.inference_time)

  // 获取AI建议
  aiAdvice.value = res.data.advice
}
</script>
```

---

### 3. `src/views/Home.vue` —— 首页仪表盘

```vue
<template>
  <div class="dashboard">
    <div class="radar-chart">
      <ECharts :option="radarOption" />
    </div>

    <div class="recent-predictions">
      <h3>近十日预测数量</h3>
      <ECharts :option="lineOption" />
    </div>

    <div class="latest-records">
      <h3>最新检测记录</h3>
      <table>
        <tr>
          <th>用户名</th>
          <th>识别权重</th>
          <th>最小阈值</th>
          <th>AI助手</th>
          <th>时间</th>
        </tr>
        <tr v-for="record in latestRecords" :key="record.id">
          <td>{{ record.username }}</td>
          <td>{{ record.model }}</td>
          <td>{{ record.threshold }}</td>
          <td>{{ record.aiHelper }}</td>
          <td>{{ record.time }}</td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const radarOption = ref({
  radar: {
    indicator: [
      { name: '飞机', max: 100 },
      { name: '篮球场', max: 100 },
      { name: '足球场', max: 100 },
      { name: '船', max: 100 },
      { name: '网球场', max: 100 }
    ]
  },
  series: [{
    type: 'radar',
    data: [{ value: [93.38, 83.7, 92.5, 89.41, 91.2] }]
  }]
})

const lineOption = ref({
  xAxis: { type: 'category', data: ['2025-08-03', '2025-08-04', ...] },
  yAxis: { type: 'value' },
  series: [{ type: 'line', data: [0, 80, 0, 10, 0, 90, 0, 0, 0, 180, 0] }]
})

const latestRecords = ref([
  { username: 'admin', model: 'best.onnx', threshold: 0.5, aiHelper: '不使用AI', time: '2025-09-23 11:54:50' },
  // 更多记录...
])
</script>
```

---

## ✅ 五、后端代码（SpringBoot + Java）

### 1. `controller/DetectionController.java`

```java
@RestController
@RequestMapping("/api")
public class DetectionController {

    @Autowired
    private DetectionService detectionService;

    @PostMapping("/detect/image")
    public ResponseEntity<DetectionResult> detectImage(@RequestParam("image") MultipartFile image) throws IOException {
        String result = detectionService.detectImage(image);
        return ResponseEntity.ok(result);
    }

    @PostMapping("/detect/video")
    public ResponseEntity<DetectionResult> detectVideo(@RequestParam("video") MultipartFile video) throws IOException {
        String result = detectionService.detectVideo(video);
        return ResponseEntity.ok(result);
    }

    @GetMapping("/records/image")
    public ResponseEntity<List<ImageRecord>> getImageRecords() {
        List<ImageRecord> records = detectionService.getImageRecords();
        return ResponseEntity.ok(records);
    }

    @GetMapping("/records/video")
    public ResponseEntity<List<VideoRecord>> getVideoRecords() {
        List<VideoRecord> records = detectionService.getVideoRecords();
        return ResponseEntity.ok(records);
    }
}
```

---

### 2. `service/DetectionService.java`

```java
@Service
public class DetectionService {

    @Autowired
    private FlaskClient flaskClient;

    @Autowired
    private DeepSeekClient deepSeekClient;

    public DetectionResult detectImage(MultipartFile image) throws IOException {
        // 1. 调用 Flask 进行 YOLO 检测
        DetectionResult yoloResult = flaskClient.detectImage(image);

        // 2. 调用 DeepSeek 生成建议
        String advice = deepSeekClient.generateAdvice(yoloResult.getDetectedObjects());

        // 3. 保存到数据库
        ImageRecord record = new ImageRecord();
        record.setUsername("admin");
        record.setModel("best.onnx");
        record.setThreshold(0.5);
        record.setAiHelper("不使用AI");
        record.setTimestamp(new Date());
        record.setOriginalImage(image.getBytes());
        record.setPredictedImage(yoloResult.getImageBase64());

        imageRepository.save(record);

        // 返回结果
        yoloResult.setAdvice(advice);
        return yoloResult;
    }
}
```

---

## ✅ 六、Python 模型训练脚本（YOLOv11）

```python
# train.py
from ultralytics import YOLO

model = YOLO('yolov11s.pt')  # 使用预训练模型

# 训练
model.train(
    data='dataset.yaml',
    epochs=100,
    imgsz=640,
    batch=16,
    name='remote_sensing_detection'
)

# 导出为 ONNX 格式（便于部署）
model.export(format='onnx')
```

---

## ✅ 七、大模型建议生成（DeepSeek / Qwen）

```python
# deepseek_client.py
import requests

def generate_advice(objects: list[str]):
    url = "https://api.deepseek.com/v1/chat/completions"
    headers = {
        "Authorization": "Bearer YOUR_API_KEY",
        "Content-Type": "application/json"
    }
    payload = {
        "model": "deepseek-chat",
        "messages": [
            {"role": "system", "content": "你是一位遥感专家，擅长分析地理空间目标"},
            {"role": "user", "content": f"""
            在本次遥感图像中检测到以下目标：
            {', '.join(objects)}
            请给出以下分析：
            1. 目标分布特征
            2. 可能用途或意义
            3. 建议进一步分析方向
            """}
        ]
    }
    response = requests.post(url, headers=headers, json=payload)
    return response.json()['choices'][0]['message']['content']
```

---

## ✅ 八、PDF 导出功能（使用 `pdfkit`）

```python
# export_pdf.py
import pdfkit
from jinja2 import Template

def export_to_pdf(result, filename):
    template = Template("""
    <html>
    <head><title>遥感检测报告</title></head>
    <body>
        <h1>遥感地理空间目标检测报告</h1>
        <p><strong>检测目标：</strong>{{ result.detected_objects }}</p>
        <p><strong>建议：</strong>{{ result.advice }}</p>
    </body>
    </html>
    """)
    html = template.render(result=result)
    pdfkit.from_string(html, filename)
```

---

## ✅ 九、部署教程

### 1. 启动 Flask 服务
```bash
cd flask_app
python flask_app.py
```

### 2. 启动 SpringBoot 服务
```bash
mvn spring-boot:run
```

### 3. 启动 Vue 前端
```bash
cd frontend
npm run serve
```

### 4. Docker 部署（可选）
```dockerfile
FROM nginx:alpine
COPY dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

---

## ✅ 十、特色功能总结

| 功能 | 说明 |
|------|------|
| 🌍 **多类遥感目标** | 支持飞机、篮球场、船等10类 |
| 🤖 **AI智能分析** | DeepSeek 自动生成专业建议 |
| 📹 **视频流处理** | 支持MP4、AVI等格式 |
| 📊 **可视化统计** | ECharts 展示趋势和雷达图 |
| 📄 **PDF导出** | 一键生成分析报告 |
| 🔐 **用户管理** | 支持多用户登录和权限控制 |

---


