# Week 3 Session 2
# Embeddings 
```
import os
import httpx


async def embed(text: str) -> list[float]:
    """Get embedding vector for text using OpenAI's API."""
    async with httpx.AsyncClient() as client:
        response = await client.post(
            "https://api.openai.com/v1/embeddings",
            headers={"Authorization": f"Bearer {os.environ['OPENAI_API_KEY']}"},
            json={"model": "text-embedding-3-small", "input": text},
        )
        return response.json()["data"][0]["embedding"] 


async def get_similarity(text1: str, text2: str) -> float:
    """Calculate cosine similarity between two texts."""
    emb1 = np.array(await embed(text1))
    emb2 = np.array(await embed(text2))
    return float(np.dot(emb1, emb2) / (np.linalg.norm(emb1) * np.linalg.norm(emb2)))

async def main():
    print(await get_similarity("Apple", "Orange"))
    print(await get_similarity("Apple", "Lightning"))


if __name__ == "__main__":
    import asyncio

    asyncio.run(main())
```

Here Apple and Orange will get the higher value showing the highest similarity as Compared to the Apple and Lightning

# Multimodel Embeddings
1. nomic Atlas

Text Embedding
```
curl -X POST "https://api-atlas.nomic.ai/v1/embedding/text" \
  -H "Authorization: Bearer $NOMIC_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
        "model": "nomic-embed-text-v1.5",
        "task_type": "search_document",
        "texts": ["A cute cat", "A cardboard box"]
      }'
```
Image Embeddings
```
curl -X POST "https://api-atlas.nomic.ai/v1/embedding/image" \
  -H "Authorization: Bearer $NOMIC_API_KEY" \
  -F "model=nomic-embed-vision-v1.5" \
  -F "images=@cat.jpg" \
  -F "images=@box.png"
```
now Comparing the embeddings 
```
import json
with open('nomic_text_response.json','r') as f:
    data = json.load(f)

with open('nomic_image_response.json','r') as f:
    data = json.load(f)

cat_img_embedding = image_response['embeddings'][0]
cat_text_embedding = text_response['embeddings'][0]
box_img_embedding = image_response['embeddings'][1]
box_text_embedding = text_response['embeddings'][1]

cat_similarity = float(np.dot(cat_img_embedding,cat_text_embedding), np,linalg.norm(cat_img_embedding)*np,linalg.norm(cat_text_embedding))
box_similarity = float(np.dot(box_img_embedding,box_text_embedding), np,linalg.norm(box_img_embedding)*np,linalg.norm(box_text_embedding))

print(cat_similarity)
print(box_similarity)
```