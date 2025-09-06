
Для точного анализа производительности функций в Python по времени выполнения и потребляемой памяти используются специализированные инструменты. Приведу профессиональные методы, применяемые в промышленной разработке.

**Анализ времени выполнения:**

1. **Модуль timeit (точные микроизмерения)**
    

python

```python
import timeit

def test_func():
    # Тело функции

time = timeit.timeit(test_func, number=1000)
print(f"Среднее время: {time/1000:.6f} сек")

```

- Минимизирует накладные расходы
    
- Подходит для бенчмарков с большим числом итераций
    
- Рекомендуется number ≥ 1000 для стабильных результатов
    

1. **Декоратор с наносекундной точностью**
    

```python
from time import perf_counter_ns

def benchmark(func):
    def wrapper(*args, **kwargs):
        start = perf_counter_ns()
        result = func(*args, **kwargs)
        end = perf_counter_ns()
        print(f"{func.__name__}: {(end - start)/1e9:.6f} сек")
        return result
    return wrapper

@benchmark
def optimized_function():
    # Тело функции

```

**Профилирование памяти:**

1. **Memory Profiler (посекционное отслеживание)**
    

```python
from memory_profiler import profile

@profile(precision=4)
def memory_intensive():
    # Код для анализа
    del temporary_object  # Явное освобождение памяти
```

- Требует запуска через `python -m memory_profiler script.py`
    
- Выводит потребление по строкам кода
    
- Опция `precision=4` для отображения 4 знаков после запятой (МБ)
    

1. **Tracemalloc (стандартная библиотека)**
    

```python
import tracemalloc

tracemalloc.start()

# Снимок памяти до выполнения
snapshot1 = tracemalloc.take_snapshot()

critical_function()

# Снимок после выполнения
snapshot2 = tracemalloc.take_snapshot()

# Анализ различий
top_stats = snapshot2.compare_to(snapshot1, 'lineno')
for stat in top_stats[:5]:
    print(stat)
```

**Продвинутые методы оптимизации:**

1. **Комбинированный анализ cProfile + Snakeviz**
    

```bash
python -m cProfile -o profile_data.prof script.py
snakeviz profile_data.prof

```

- Визуализация в браузере с детализацией по вызовам
    
- Выявление скрытых рекурсий и узких мест
    

1. **Статический анализ памяти с objgraph**
    

```python

import objgraph

def leak_detector():
    before = objgraph.typestats()
    suspicious_function()
    after = objgraph.typestats()
    print(objgraph.show_growth(initial=before, peak=after))

```


**Рекомендации для точных измерений:**

1. Проводите замеры в изолированном окружении (отдельный процесс)
    
2. Отключайте сборщик мусора во время тестов:
    
```python
import gc
gc.disable()
```
    
    
1. Для функций с побочными эффектами используйте глубокое копирование входных данных
    
4. При анализе JIT-кода (PyPy) выполняйте предварительный прогрев
    
5. Для многопоточных приложений учитывайте contention points через `threading`-профилирование
    

Пример комплексного решения:

```python
from functools import wraps
import time
import tracemalloc
import gc

def performance_analyzer(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        gcold = gc.isenabled()
        gc.disable()
        
        tracemalloc.start()
        start_time = time.perf_counter_ns()
        
        result = func(*args, **kwargs)
        
        elapsed = (time.perf_counter_ns() - start_time) / 1e9
        current, peak = tracemalloc.get_traced_memory()
        tracemalloc.stop()
        
        if gcold:
            gc.enable()
            
        print(f"[{func.__name__}] Время: {elapsed:.4f} сек")
        print(f"Пиковая память: {peak / 1e6:.2f} МБ")
        return result
        
    return wrapper

```


Для промышленного использования рассмотрите:

- Py-Spy для sampling-профилирования без модификации кода
    
- Scalene для интегрального анализа CPU/GPU/памяти
    
- Memray для трекинга аллокаций в C-расширениях
    

Все методы должны применяться на репрезентативных рабочих нагрузках, приближенных к реальным данным приложения. Для CI/CD-интеграции используйте pytest-benchmark с историей измерений.