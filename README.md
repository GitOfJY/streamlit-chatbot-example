# 🤖 소득세 챗봇

한국 소득세법에 대해 질문하면 AI가 관련 법 조항을 찾아 답변해주는 RAG 기반 챗봇입니다.

**배포:** https://app-chatbot-example.streamlit.app/

## 주요 기능

- 소득세법 문서 기반 RAG (Retrieval-Augmented Generation) 답변
- 대화 이력을 반영한 맥락 유지 (History-Aware Retriever)
- Few-Shot Prompting으로 답변 형식 통일 ("소득세법 제XX조에 따르면...")
- 실시간 스트리밍 응답

## 기술 스택

| 구분 | 기술 |
|------|------|
| Frontend | Streamlit |
| LLM | OpenAI GPT-4o |
| Embedding | OpenAI text-embedding-3-large |
| Vector DB | Pinecone |
| Framework | LangChain (LCEL) |
| 배포 | Streamlit Cloud |

## 아키텍처

```
사용자 입력
  → Dictionary Chain (질문 전처리: "사람" → "거주자" 변환)
  → History-Aware Retriever (대화 이력 기반 질문 재구성)
  → Pinecone 벡터 검색 (소득세법 문서에서 유사 문서 4개 검색)
  → Stuff Documents Chain (검색된 문서 + 질문을 GPT-4o에 전달)
  → 스트리밍 응답 출력
```

## 프로젝트 구조

```
├── chat.py           # Streamlit UI, 세션 관리, 메시지 제한
├── llm.py            # LangChain RAG 파이프라인 구성
├── config.py         # Few-Shot 예시 데이터
├── requirements.txt  # 의존성 패키지
└── .gitignore
```

## 로컬 실행

```bash
git clone https://github.com/GitOfJY/streamlit-chatbot-example.git
cd streamlit-chatbot-example
python -m venv venv
venv\Scripts\activate  # Windows
pip install -r requirements.txt
```

`.env` 파일 생성:

```
OPENAI_API_KEY=your-key
PINECONE_API_KEY=your-key
```

```bash
streamlit run chat.py
```