# AI-Agent-
[简历解析师] → [岗位匹配官] → [面试设计师] → [面试教练] → [评估委员会] → [薪酬谈判顾问]
# create_project.py
"""
AI 影视剧本工厂 — 一键生成完整项目文件
运行方式: python create_project.py
"""
import os

BASE_DIR = "ai-script-factory"
BACKEND = os.path.join(BASE_DIR, "backend", "app")
FRONTEND = os.path.join(BASE_DIR, "frontend", "src", "components")

FILES = {
    # backend
    "backend/requirements.txt": "fastapi==0.115.0\nuvicorn==0.30.6\nsqlalchemy==2.0.35\npydantic==2.9.2\npython-multipart==0.0.9\n",
    "backend/run.py": "import uvicorn\n\nif __name__ == \"__main__\":\n    uvicorn.run(\"app.main:app\", host=\"0.0.0.0\", port=8000, reload=True)\n",
    "backend/app/__init__.py": "",
    "backend/app/core/__init__.py": "",
    "backend/app/core/config.py": "class Settings:\n    APP_NAME = \"AI Script Factory\"\n    DB_URL = \"sqlite:///./ai_script_factory.db\"\n\nsettings = Settings()\n",
    "backend/app/core/db.py": (
        "from sqlalchemy import create_engine\n"
        "from sqlalchemy.orm import sessionmaker, declarative_base\n"
        "from app.core.config import settings\n\n"
        "engine = create_engine(settings.DB_URL, connect_args={\"check_same_thread\": False})\n"
        "SessionLocal = sessionmaker(bind=engine, autoflush=False, autocommit=False)\n"
        "Base = declarative_base()\n\n"
        "def get_db():\n"
        "    db = SessionLocal()\n"
        "    try:\n"
        "        yield db\n"
        "    finally:\n"
        "        db.close()\n"
    ),
    "backend/app/models/__init__.py": "",
    "backend/app/models/project.py": (
        "from sqlalchemy import Column, Integer, String, Text, DateTime\n"
        "from datetime import datetime\n"
        "from app.core.db import Base\n\n"
        "class ScriptProject(Base):\n"
        "    __tablename__ = \"script_projects\"\n\n"
        "    id = Column(Integer, primary_key=True, index=True)\n"
        "    title = Column(String(255), nullable=False)\n"
        "    genre = Column(String(100), nullable=True)\n"
        "    theme = Column(String(255), nullable=True)\n"
        "    style = Column(String(100), nullable=True)\n"
        "    duration = Column(String(50), nullable=True)\n"
        "    outline = Column(Text, default=\"\")\n"
        "    characters = Column(Text, default=\"\")\n"
        "    scenes = Column(Text, default=\"\")\n"
        "    dialogues = Column(Text, default=\"\")\n"
        "    storyboard = Column(Text, default=\"\")\n"
        "    review = Column(Text, default=\"\")\n"
        "    final_script = Column(Text, default=\"\")\n"
        "    status = Column(String(50), default=\"created\")\n"
        "    created_at = Column(DateTime, default=datetime.utcnow)\n"
    ),
    "backend/app/schemas/__init__.py": "",
    "backend/app/schemas/project.py": (
        "from pydantic import BaseModel\n"
        "from typing import Optional\n\n"
        "class ProjectCreate(BaseModel):\n"
        "    title: str\n"
        "    genre: Optional[str] = \"\"\n"
        "    theme: Optional[str] = \"\"\n"
        "    style: Optional[str] = \"\"\n"
        "    duration: Optional[str] = \"\"\n\n"
        "class ProjectResponse(BaseModel):\n"
        "    id: int\n"
        "    title: str\n"
        "    genre: Optional[str]\n"
        "    theme: Optional[str]\n"
        "    style: Optional[str]\n"
        "    duration: Optional[str]\n"
        "    outline: str\n"
        "    characters: str\n"
        "    scenes: str\n"
        "    dialogues: str\n"
        "    storyboard: str\n"
        "    review: str\n"
        "    final_script: str\n"
        "    status: str\n\n"
        "    class Config:\n"
        "        from_attributes = True\n"
    ),
    "backend/app/agents/__init__.py": "",
    "backend/app/agents/base.py": (
        "class BaseAgent:\n"
        "    name = \"BaseAgent\"\n\n"
        "    def run(self, context: dict) -> str:\n"
        "        raise NotImplementedError(\"Agent must implement run method.\")\n"
    ),
    "backend/app/agents/planner_agent.py": (
        "from app.agents.base import BaseAgent\n\n"
        "class PlannerAgent(BaseAgent):\n"
        "    name = \"PlannerAgent\"\n\n"
        "    def run(self, context: dict) -> str:\n"
        "        return f\"\"\"\n"
        "【故事大纲】\n"
        "片名：《{context.get('title', '')}》\n"
        "类型：{context.get('genre', '')}\n"
        "主题：{context.get('theme', '')}\n"
        "风格：{context.get('style', '')}\n"
        "时长：{context.get('duration', '')}\n\n"
        "故事讲述一个围绕“{context.get('theme', '')}”展开的影视作品。\n"
        "主角在复杂环境中面对抉择、冲突与成长，逐步揭开事件真相，\n"
        "并最终完成自我和命运的对抗。\n"
        "        \"\"\".strip()\n"
    ),
    "backend/app/agents/character_agent.py": (
        "from app.agents.base import BaseAgent\n\n"
        "class CharacterAgent(BaseAgent):\n"
        "    name = \"CharacterAgent\"\n\n"
        "    def run(self, context: dict) -> str:\n"
        "        return \"\"\"\n"
        "【角色设定】\n\n"
        "1. 主角：林然，28岁，冷静、执拗、富有同理心。核心目标：寻找真相并证明自己。人物弧光：从自我怀疑到坚定担当。\n\n"
        "2. 女主：苏晚，27岁，聪慧、敏锐、克制。核心目标：守护重要秘密。人物弧光：从防备到信任。\n\n"
        "3. 反派：周崇，45岁，沉稳、强势、控制欲强。核心目标：掩盖真相，维持既得利益。人物弧光：从掌控局势到全面失控。\n"
        "        \"\"\".strip()\n"
    ),
    "backend/app/agents/scene_agent.py": (
        "from app.agents.base import BaseAgent\n\n"
        "class SceneAgent(BaseAgent):\n"
        "    name = \"SceneAgent\"\n\n"
        "    def run(self, context: dict) -> str:\n"
        "        return \"\"\"\n"
        "【分场剧情】\n\n"
        "第一场：引子——主角林然在一次意外中得到关键线索。\n"
        "第二场：相遇——林然与苏晚相识，被迫合作。\n"
        "第三场：调查——深入事件核心，发现更大阴谋。\n"
        "第四场：背叛——关键证据丢失，主角陷入低谷。\n"
        "第五场：真相逼近——林然重新振作，再次联手。\n"
        "第六场：终局对决——正面冲突，揭露真相并完成成长。\n"
        "        \"\"\".strip()\n"
    ),
    "backend/app/agents/dialogue_agent.py": (
        "from app.agents.base import BaseAgent\n\n"
        "class DialogueAgent(BaseAgent):\n"
        "    name = \"DialogueAgent\"\n\n"
        "    def run(self, context: dict) -> str:\n"
        "        return \"\"\"\n"
        "【详细剧本对白】\n\n"
        "场景一：深夜，旧仓库外\n"
        "林然：（盯着手机里的照片）“如果这张照片是真的，那一切都不是意外。”\n"
        "苏晚：（从阴影中走出）“你查到这里，已经很危险了。”\n"
        "林然：“你是谁？”\n"
        "苏晚：“一个不想让你白白送命的人。”\n\n"
        "场景二：天台\n"
        "林然：“你早就知道真相，对吗？”\n"
        "苏晚：“知道一部分。但有些真相，不是知道了就能说出来。”\n"
        "林然：“可如果所有人都沉默，那真相永远不会出现。”\n"
        "        \"\"\".strip()\n"
    ),
    "backend/app/agents/storyboard_agent.py": (
        "from app.agents.base import BaseAgent\n\n"
        "class StoryboardAgent(BaseAgent):\n"
        "    name = \"StoryboardAgent\"\n\n"
        "    def run(self, context: dict) -> str:\n"
        "        return \"\"\"\n"
        "【分镜建议】\n\n"
        "镜头1：城市夜景航拍，建立悬疑氛围。\n"
        "镜头2：近景拍摄林然查看手机照片，突出紧张情绪。\n"
        "镜头3：苏晚从暗处进入画面，侧逆光增强神秘感。\n"
        "镜头4：天台对话使用中近景与反打，突出心理博弈。\n"
        "镜头5：终局对决采用快速剪辑与手持镜头，增强冲突感。\n"
        "        \"\"\".strip()\n"
    ),
    "backend/app/agents/review_agent.py": (
        "from app.agents.base import BaseAgent\n\n"
        "class ReviewAgent(BaseAgent):\n"
        "    name = \"ReviewAgent\"\n\n"
        "    def run(self, context: dict) -> str:\n"
        "        return \"\"\"\n"
        "【审校建议】\n\n"
        "1. 故事结构完整，适合短剧或网络电影开发。\n"
        "2. 建议增强反派动机，使其行为逻辑更有说服力。\n"
        "3. 可进一步补充主角前史，以增强情感共鸣。\n"
        "4. 对白风格较克制，适合悬疑现实题材。\n"
        "5. 分镜具备影视执行基础，适合进入分镜脚本细化阶段。\n"
        "        \"\"\".strip()\n"
    ),
    "backend/app/agents/director_agent.py": (
        "from app.agents.base import BaseAgent\n\n"
        "class DirectorAgent(BaseAgent):\n"
        "    name = \"DirectorAgent\"\n\n"
        "    def run(self, context: dict) -> str:\n"
        "        return f\"\"\"\n"
        "【最终汇总剧本】\n\n"
        "{context.get('outline', '')}\n\n"
        "{context.get('characters', '')}\n\n"
        "{context.get('scenes', '')}\n\n"
        "{context.get('dialogues', '')}\n\n"
        "{context.get('storyboard', '')}\n\n"
        "{context.get('review', '')}\n"
        "        \"\"\".strip()\n"
    ),
    "backend/app/services/__init__.py": "",
    "backend/app/services/orchestration.py": (
        "from app.agents.planner_agent import PlannerAgent\n"
        "from app.agents.character_agent import CharacterAgent\n"
        "from app.agents.scene_agent import SceneAgent\n"
        "from app.agents.dialogue_agent import DialogueAgent\n"
        "from app.agents.storyboard_agent import StoryboardAgent\n"
        "from app.agents.review_agent import ReviewAgent\n"
        "from app.agents.director_agent import DirectorAgent\n\n"
        "class ScriptOrchestrator:\n"
        "    def __init__(self):\n"
        "        self.planner = PlannerAgent()\n"
        "        self.character = CharacterAgent()\n"
        "        self.scene = SceneAgent()\n"
        "        self.dialogue = DialogueAgent()\n"
        "        self.storyboard = StoryboardAgent()\n"
        "        self.review = ReviewAgent()\n"
        "        self.director = DirectorAgent()\n\n"
        "    def run(self, project_data: dict):\n"
        "        context = dict(project_data)\n"
        "        context[\"outline\"] = self.planner.run(context)\n"
        "        context[\"characters\"] = self.character.run(context)\n"
        "        context[\"scenes\"] = self.scene.run(context)\n"
        "        context[\"dialogues\"] = self.dialogue.run(context)\n"
        "        context[\"storyboard\"] = self.storyboard.run(context)\n"
        "        context[\"review\"] = self.review.run(context)\n"
        "        context[\"final_script\"] = self.director.run(context)\n"
        "        return context\n"
    ),
    "backend/app/api/__init__.py": "",
    "backend/app/api/project.py": (
        "from fastapi import APIRouter, Depends, HTTPException\n"
        "from sqlalchemy.orm import Session\n"
        "from app.core.db import get_db\n"
        "from app.models.project import ScriptProject\n"
        "from app.schemas.project import ProjectCreate, ProjectResponse\n"
        "from app.services.orchestration import ScriptOrchestrator\n\n"
        "router = APIRouter(prefix=\"/projects\", tags=[\"projects\"])\n\n"
        "@router.post(\"/\", response_model=ProjectResponse)\n"
        "def create_project(payload: ProjectCreate, db: Session = Depends(get_db)):\n"
        "    project = ScriptProject(\n"
        "        title=payload.title, genre=payload.genre,\n"
        "        theme=payload.theme, style=payload.style,\n"
        "        duration=payload.duration, status=\"processing\"\n"
        "    )\n"
        "    db.add(project)\n"
        "    db.commit()\n"
        "    db.refresh(project)\n\n"
        "    orchestrator = ScriptOrchestrator()\n"
        "    result = orchestrator.run({\n"
        "        \"title\": project.title, \"genre\": project.genre,\n"
        "        \"theme\": project.theme, \"style\": project.style,\n"
        "        \"duration\": project.duration\n"
        "    })\n\n"
        "    project.outline = result[\"outline\"]\n"
        "    project.characters = result[\"characters\"]\n"
        "    project.scenes = result[\"scenes\"]\n"
        "    project.dialogues = result[\"dialogues\"]\n"
        "    project.storyboard = result[\"storyboard\"]\n"
        "    project.review = result[\"review\"]\n"
        "    project.final_script = result[\"final_script\"]\n"
        "    project.status = \"completed\"\n\n"
        "    db.commit()\n"
        "    db.refresh(project)\n"
        "    return project\n\n"
        "@router.get(\"/\", response_model=list[ProjectResponse])\n"
        "def list_projects(db: Session = Depends(get_db)):\n"
        "    return db.query(ScriptProject).order_by(ScriptProject.id.desc()).all()\n\n"
        "@router.get(\"/{project_id}\", response_model=ProjectResponse)\n"
        "def get_project(project_id: int, db: Session = Depends(get_db)):\n"
        "    project = db.query(ScriptProject).filter(ScriptProject.id == project_id).first()\n"
        "    if not project:\n"
        "        raise HTTPException(status_code=404, detail=\"Project not found\")\n"
        "    return project\n"
    ),
    "backend/app/main.py": (
        "from fastapi import FastAPI\n"
        "from fastapi.middleware.cors import CORSMiddleware\n"
        "from app.core.db import Base, engine\n"
        "from app.api.project import router as project_router\n"
        "from app.core.config import settings\n\n"
        "Base.metadata.create_all(bind=engine)\n\n"
        "app = FastAPI(title=settings.APP_NAME)\n\n"
        "app.add_middleware(\n"
        "    CORSMiddleware,\n"
        "    allow_origins=[\"*\"],\n"
        "    allow_credentials=True,\n"
        "    allow_methods=[\"*\"],\n"
        "    allow_headers=[\"*\"],\n"
        ")\n\n"
        "app.include_router(project_router)\n\n"
        "@app.get(\"/\")\n"
        "def root():\n"
        "    return {\"message\": \"AI Script Factory Backend Running\"}\n"
    ),
    # frontend files
    "frontend/package.json": (
        '{\n'
        '  "name": "ai-script-factory-frontend",\n'
        '  "version": "1.0.0",\n'
        '  "private": true,\n'
        '  "scripts": {\n'
        '    "dev": "vite",\n'
        '    "build": "vite build"\n'
        '  },\n'
        '  "dependencies": {\n'
        '    "axios": "^1.7.7",\n'
        '    "element-plus": "^2.8.4",\n'
        '    "vue": "^3.5.10"\n'
        '  },\n'
        '  "devDependencies": {\n'
        '    "@vitejs/plugin-vue": "^5.1.4",\n'
        '    "vite": "^5.4.8"\n'
        '  }\n'
        '}\n'
    ),
    "frontend/vite.config.js": (
        "import { defineConfig } from 'vite'\n"
        "import vue from '@vitejs/plugin-vue'\n\n"
        "export default defineConfig({\n"
        "  plugins: [vue()],\n"
        "  server: {\n"
        "    port: 5173\n"
        "  }\n"
        "})\n"
    ),
    "frontend/index.html": (
        '<!DOCTYPE html>\n<html lang="en">\n<head>\n  <meta charset="UTF-8" />\n  <meta name="viewport" content="width=device-width, initial-scale=1.0" />\n  <title>AI Script Factory</title>\n</head>\n<body>\n  <div id="app"></div>\n  <script type="module" src="/src/main.js"></script>\n</body>\n</html>\n'
    ),
    "frontend/src/main.js": (
        "import { createApp } from 'vue'\n"
        "import App from './App.vue'\n"
        "import ElementPlus from 'element-plus'\n"
        "import 'element-plus/dist/index.css'\n\n"
        "createApp(App).use(ElementPlus).mount('#app')\n"
    ),
    "frontend/src/App.vue": (
        '<template>\n'
        '  <div style="max-width: 1200px; margin: 30px auto;">\n'
        '    <h1>AI 影视剧本工厂 Agent 协同运营自动化系统</h1>\n'
        '    <ProjectForm @created="reloadList" />\n'
        '    <ProjectList :refreshKey="refreshKey" />\n'
        '  </div>\n'
        '</template>\n\n'
        '<script setup>\n'
        'import { ref } from \'vue\'\n'
        'import ProjectForm from \'./components/ProjectForm.vue\'\n'
        'import ProjectList from \'./components/ProjectList.vue\'\n\n'
        'const refreshKey = ref(0)\n'
        'const reloadList = () => {\n'
        '  refreshKey.value++\n'
        '}\n'
        '</script>\n'
    ),
    "frontend/src/components/ProjectForm.vue": (
        '<template>\n'
        '  <el-card style="margin-bottom: 20px;">\n'
        '    <template #header>创建剧本项目</template>\n'
        '    <el-form :model="form" label-width="100px">\n'
        '      <el-form-item label="标题"><el-input v-model="form.title" /></el-form-item>\n'
        '      <el-form-item label="类型"><el-input v-model="form.genre" placeholder="悬疑 / 爱情 / 科幻" /></el-form-item>\n'
        '      <el-form-item label="主题"><el-input v-model="form.theme" placeholder="真相、救赎、成长" /></el-form-item>\n'
        '      <el-form-item label="风格"><el-input v-model="form.style" placeholder="现实主义、商业片、黑色电影" /></el-form-item>\n'
        '      <el-form-item label="时长"><el-input v-model="form.duration" placeholder="30分钟 / 90分钟" /></el-form-item>\n'
        '      <el-form-item><el-button type="primary" @click="submit" :loading="loading">生成剧本</el-button></el-form-item>\n'
        '    </el-form>\n'
        '  </el-card>\n'
        '</template>\n\n'
        '<script setup>\n'
        'import axios from \'axios\'\n'
        'import { reactive, ref } from \'vue\'\n'
        'import { ElMessage } from \'element-plus\'\n\n'
        'const emit = defineEmits([\'created\'])\n'
        'const loading = ref(false)\n'
        'const form = reactive({\n'
        '  title: \'\', genre: \'\', theme: \'\', style: \'\', duration: \'\'\n'
        '})\n\n'
        'const submit = async () => {\n'
        '  if (!form.title) { ElMessage.error(\'请输入标题\'); return }\n'
        '  loading.value = true\n'
        '  try {\n'
        '    await axios.post(\'http://127.0.0.1:8000/projects/\', form)\n'
        '    ElMessage.success(\'剧本生成成功\')\n'
        '    Object.assign(form, { title: \'\', genre: \'\', theme: \'\', style: \'\', duration: \'\' })\n'
        '    emit(\'created\')\n'
        '  } catch (e) { ElMessage.error(\'生成失败\') }\n'
        '  finally { loading.value = false }\n'
        '}\n'
        '</script>\n'
    ),
    "frontend/src/components/ProjectList.vue": (
        '<template>\n'
        '  <el-card>\n'
        '    <template #header>项目列表</template>\n'
        '    <el-collapse>\n'
        '      <el-collapse-item v-for="item in projects" :key="item.id" :title="`${item.id}. ${item.title} [${item.status}]`" :name="item.id">\n'
        '        <p><b>类型：</b>{{ item.genre }}</p>\n'
        '        <p><b>主题：</b>{{ item.theme }}</p>\n'
        '        <p><b>风格：</b>{{ item.style }}</p>\n'
        '        <p><b>时长：</b>{{ item.duration }}</p>\n'
        '        <el-divider>故事大纲</el-divider><pre>{{ item.outline }}</pre>\n'
        '        <el-divider>角色设定</el-divider><pre>{{ item.characters }}</pre>\n'
        '        <el-divider>分场剧情</el-divider><pre>{{ item.scenes }}</pre>\n'
        '        <el-divider>详细对白</el-divider><pre>{{ item.dialogues }}</pre>\n'
        '        <el-divider>分镜建议</el-divider><pre>{{ item.storyboard }}</pre>\n'
        '        <el-divider>审校建议</el-divider><pre>{{ item.review }}</pre>\n'
        '        <el-divider>最终剧本</el-divider><pre>{{ item.final_script }}</pre>\n'
        '      </el-collapse-item>\n'
        '    </el-collapse>\n'
        '  </el-card>\n'
        '</template>\n\n'
        '<script setup>\n'
        'import axios from \'axios\'\n'
        'import { ref, watch, onMounted } from \'vue\'\n'
        'import { ElMessage } from \'element-plus\'\n\n'
        'const props = defineProps({ refreshKey: Number })\n'
        'const projects = ref([])\n'
        'const loadData = async () => {\n'
        '  try {\n'
        '    const res = await axios.get(\'http://127.0.0.1:8000/projects/\')\n'
        '    projects.value = res.data\n'
        '  } catch (e) { ElMessage.error(\'加载项目失败\') }\n'
        '}\n'
        'watch(() => props.refreshKey, loadData)\n'
        'onMounted(loadData)\n'
        '</script>\n'
    ),
    "README.md": (
        "# AI 影视剧本工厂 Agent 协同运营自动化系统\n\n"
        "## 快速开始\n\n"
        "### 1. 后端启动\n```bash\ncd backend\npip install -r requirements.txt\npython run.py\n```\n"
        "后端运行在 http://127.0.0.1:8000\n\n"
        "### 2. 前端启动\n```bash\ncd frontend\nnpm install\nnpm run dev\n```\n"
        "前端运行在 http://127.0.0.1:5173\n\n"
        "## 功能\n"
        "- 创建剧本项目并自动多Agent生成\n"
        "- 查看历史剧本详情\n"
        "- 后续可接入真实LLM API\n"
    )
}

def main():
    for path, content in FILES.items():
        full = os.path.join(BASE_DIR, path)
        os.makedirs(os.path.dirname(full), exist_ok=True)
        with open(full, "w", encoding="utf-8") as f:
            f.write(content)
    print(f"✅ 项目已生成在 {os.path.abspath(BASE_DIR)}")

if __name__ == "__main__":
    main()
