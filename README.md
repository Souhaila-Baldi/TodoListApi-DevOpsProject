from fastapi import FastAPI, HTTPException
import logging
import time

# Create FastAPI application instance
app = FastAPI()

# Logging Configuration 
logging.basicConfig(level=logging.INFO)
logger = logging.getLogger("todo-api")


tasks = []
request_count = 0  

# Middleware for Logging and Metrics
@app.middleware("http")
async def log_requests(request, call_next):
    """
    Middleware that:
    - Counts incoming HTTP requests (metrics)
    - Measures request processing time
    - Logs request path and response duration
    """
    global request_count
    request_count += 1
    start_time = time.time()
    
    response = await call_next(request)
    
    duration = time.time() - start_time
    logger.info(f"Path: {request.url.path} | Duration: {duration:.4f}s")
    
    return response

# API Endpoints 
@app.get("/tasks")
def get_tasks():
    """
    Retrieve all tasks
    """
    return tasks

@app.post("/tasks")
def create_task(task: dict):
    """
    Create a new task
    Each task must contain a 'title'
    """
    if "title" not in task:
        raise HTTPException(status_code=400, detail="Title missing")
    
    task["id"] = len(tasks) + 1
    tasks.append(task)
    return task

# Observability Endpoint 
@app.get("/metrics")
def metrics():
    """
    Expose simple application metrics:
    - total_requests: number of received HTTP requests
    - active_tasks: number of tasks currently stored
    """
    return {
        "total_requests": request_count,
        "active_tasks": len(tasks)
    }

# Health Check Endpoint 
@app.get("/health")
def health():
    """
    Health check endpoint used by Kubernetes liveness probe
    """
    return {"status": "healthy"}
