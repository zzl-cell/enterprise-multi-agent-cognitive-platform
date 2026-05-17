# ==========================
# 核心技术笔记 | 项目核心代码
# 对应上文：监控采集 + Exporter + 故障注入 + 配置管理
# ==========================

# 1. 系统指标采集核心（psutil 基础）
import psutil
import time

def get_core_metrics():
    """采集CPU/内存/系统负载核心代码"""
    cpu = psutil.cpu_percent(interval=1)
    mem = psutil.virtual_memory().percent
    load = round(psutil.getloadavg()[0], 2)
    return cpu, mem, load

# 2. Prometheus Exporter 基础服务
from flask import Flask
app = Flask(__name__)

@app.route("/metrics")
def metrics_api():
    """暴露指标接口（对接Prometheus）"""
    cpu, mem, load = get_core_metrics()
    return (
        f"# 核心监控指标\n"
        f"cpu_usage {cpu}\n"
        f"mem_usage {mem}\n"
        f"system_load {load}\n"
    )

# 3. 配置热加载基础（.env 配置读取）
def load_config():
    """加载环境配置，支持热重载"""
    config = {}
    with open(".env", "r", encoding="utf-8") as f:
        for line in f.readlines():
            line = line.strip()
            if line and not line.startswith("#"):
                k, v = line.split("=", 1)
                config[k] = v
    return config

# 4. 故障注入核心代码（混沌工程）
import multiprocessing
import os

def cpu_fault_inject():
    """CPU 满载故障注入"""
    p = multiprocessing.Process(target=os.system, args=("stress-ng --cpu 0 -q",))
    p.start()

# 5. 服务主入口（整合所有核心技术）
if __name__ == "__main__":
    # 加载配置
    config = load_config()
    print("配置加载完成:", config)
    
    # 启动 Exporter 服务
    app.run(host="0.0.0.0", port=9100)
