# Artifacts Directory

Эта папка содержит артефакты сессий режима **Dual Design** - коллаборативного проектирования с двумя AI-моделями.

## Структура

Каждая сессия создает отдельную подпапку:

```
artifacts/
  session_20251114_084500/
    state.json                        # Состояние сессии
    step1_user_request.md             # Исходный запрос пользователя
    step1.5_web_research.md           # Веб-исследование (опционально) ⭐ v1.1.0
    step2_architect_questions.md      # Вопросы Архитектора
    step3_pilot_questions.md          # Вопросы Второго пилота
    step4_architect_analysis.md       # Анализ Архитектора
    step5_pilot_recommendations.md    # Рекомендации Пилота
    step6_mvp_questions.md            # MVP вопросы
    step7_pilot_improvements.md       # Улучшения Пилота
    step8_final_questions.md          # Финальные вопросы для пользователя
    step9_user_answers.md             # Ответы пользователя
    business_requirements.md          # ФИНАЛЬНЫЙ РЕЗУЛЬТАТ
```

## Жизненный цикл сессии

### Этап 1: Инициализация (Шаг 1)
- Создается папка `session_[timestamp]/`
- Инициализируется `state.json`
- Сохраняется исходный запрос пользователя

### Этап 2: Коллаборация (Шаги 2-8)
- Два AI-агента обмениваются мнениями
- Каждый шаг сохраняется в отдельный файл
- State обновляется после каждого шага

### Этап 3: Финализация (Шаг 9)
- Пользователь отвечает на финальные вопросы
- Генерируется документ бизнес-требований
- Сессия завершается

## State.json

Файл `state.json` отслеживает прогресс сессии:

```json
{
  "currentStep": 4,
  "sessionId": "session_20251114_084500",
  "model1TaskId": "task-uuid-model1",
  "model2TaskId": "task-uuid-model2",
  "userRequest": "Создать обработку для синхронизации номенклатуры",
  "artifactsPath": "artifacts/session_20251114_084500/",
  "timestamp": "2025-11-14T08:45:00Z",
  "nextAction": "send_to_model2",
  "webResearchEnabled": true,
  "webResearchCompleted": true,
  "mcpToolsAvailable": ["web_search_exa"],
  "searchQueriesExecuted": [
    "Wildberries API документация seller",
    "1C интеграция Wildberries best practices"
  ]
}
```

## Использование артефактов

### Просмотр прогресса
```bash
# Посмотреть список сессий
ls artifacts/

# Посмотреть состояние конкретной сессии
cat artifacts/session_20251114_084500/state.json

# Посмотреть финальный результат
cat artifacts/session_20251114_084500/business_requirements.md
```

### Повторное использование
- Артефакты можно использовать для анализа процесса принятия решений
- Промежуточные результаты можно редактировать и использовать в новых сессиях
- Финальные требования готовы для передачи в разработку

### Очистка старых сессий
```bash
# Удалить сессии старше 30 дней (пример)
find artifacts/ -name "session_*" -type d -mtime +30 -exec rm -rf {} +
```

## Формат файлов

Все файлы в формате Markdown для удобства чтения и редактирования.

### Шаблон step файла
```markdown
# [Название шага]

**Timestamp:** YYYY-MM-DD HH:MM:SS
**Model:** Architect / Co-pilot
**Status:** completed / in-progress / failed

## Контекст
[Входные данные для этого шага]

## Результат
[Выход модели]

## Метаданные
- Task ID: [UUID]
- Duration: [время выполнения]
```

## Интеграция с другими режимами

После завершения сессии Dual Design, бизнес-требования можно использовать:

1. **design-1c-object** - для проектирования технической спецификации
2. **plantasks1c** - для создания плана разработки
3. **code1c** - для реализации функционала

Пример workflow:
```
Dual Design → business_requirements.md
    ↓
design-1c-object → design.xml
    ↓
plantasks1c → TODO.md
    ↓
code1c → реализация
```

## Безопасность и конфиденциальность

⚠️ **ВАЖНО:** Артефакты могут содержать конфиденциальную информацию:
- Бизнес-требования
- Технические детали
- Коммерческие данные

Рекомендации:
- Добавьте `artifacts/` в `.gitignore` если работаете с закрытыми данными
- Регулярно очищайте старые сессии
- Используйте шифрование для чувствительных данных