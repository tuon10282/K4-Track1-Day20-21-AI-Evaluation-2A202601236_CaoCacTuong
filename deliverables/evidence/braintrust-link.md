# LangSmith Trace Link — VLearn AI Tutor Evaluation

- **Platform:** LangSmith (smith.langchain.com)
- **Project Name:** `ai-evaluation`
- **Direct Project URL:** https://smith.langchain.com/o/3129b950-83a2-46c0-8cb4-eb41983a5cd4/projects/p/5ddcbc7b-73e6-4903-9808-e5ca3062d03e
- **Project ID:** `5ddcbc7b-73e6-4903-9808-e5ca3062d03e`
- **Tenant ID (Organization):** `3129b950-83a2-46c0-8cb4-eb41983a5cd4`
- **API Key Configuration:** Đã cấu hình biến `LANGSMITH_API_KEY` trong `.env`.
- **Trace Coverage & Details:**
  - Mọi lượt chạy đánh giá của tutor (`eval/run_eval.py`) và LLM judge (`eval/judge.py`) được tự động gắn trace qua `eval/tracing.py`.
  - Mỗi trace ghi lại đầy đủ: inputs (câu hỏi học viên + slide context), outputs (JSON phản hồi contract), tool calls (`kb_search` truy vấn và kết quả trả về), latency (thời gian phản hồi) và token usage / cost.
