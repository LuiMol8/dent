```markdown
# Proyecto dent

Sistema de detección y predicción temprana de enfermedades dentales usando YOLOv8.

## Requisitos
- Python 3.8+
- GPU con CUDA (recomendado) o CPU (lento)
- Visual Studio Code

## Instalación
1. Clona o copia la carpeta `dent/` en tu máquina.
2. Crea y activa un entorno virtual:

```bash
python -m venv .venv
# Windows
.\.venv\Scripts\activate
# Linux/Mac
source .venv/bin/activate
```

3. Instala dependencias:

```bash
pip install -r requirements.txt
```

4. Ajusta `yolo_dataset.yaml` con rutas y clases.

## Entrenamiento (ejemplo)
```bash
cd src
python train_yolo.py --cfg ../yolo_dataset.yaml --epochs 100 --batch 8 --imgsz 640 --pretrained yolov8s.pt --save_dir ../runs
```

## Inferencia local
```bash
python predict.py --weights ../runs/dental_yolo/weights/best.pt --source ../data/images/test --conf 0.3 --save_dir ../preds
```

## API (FastAPI)
```bash
uvicorn src.app:app --host 0.0.0.0 --port 8000 --reload
```

## Docker
Construir y ejecutar (ajusta rutas de pesos):
```bash
docker build -t dent-yolo:latest .
docker run -p 8000:8000 -v /ruta/local/pesos:/app/runs dent-yolo:latest
```

## Notas
- Asegúrate de que `data/images/*` y `data/labels/*` estén organizados en subcarpetas `train/ val/ test/`.
- Ajusta `yolo_dataset.yaml` con el número de clases y nombres reales.
- Si usas DICOM, usa `src/dataset_utils.py::convert_dicom_to_png` antes de entrenar.
```