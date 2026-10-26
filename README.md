[test_api.py](https://github.com/user-attachments/files/33959109/test_api.py)
# AI-Guess-number-gameimport random
from google import genai

client = genai.Client(api_key='YOUR_API_KEY') 

secret_num = random.randint(1, 100)
guess = None
guess_count = 0
guess_limit = 5      
out_of_limit = False
history_gusses = []
print("🤖 Welcom to AI 猜數字遊戲！range 1~100")

while secret_num != guess and not(out_of_limit):
    if guess_count < guess_limit:
        try:

            guess = int(input("\n請輸入lucky number："))
            guess_count += 1
            remain_times = guess_limit - guess_count
            history_gusses.append(guess)
            if guess == secret_num:
                print("🎉 congratulations 你猜對了！You win AI！")
            else:
                print("🤖 AI is thinking...")
                
                prompt = f"""
                玩家正在玩猜數字(1-100)。答案是 {secret_num}，但玩家猜了 {guess}。
                他現在只剩下 {remain_times} 次機會。
                請你扮演一個有點搞笑毒舌但又會給提示的 AI 小幫手，用一句話回覆玩家。
                注意：
                1. 絕對不可以把答案說出來！
                2. 必須提示玩家該往「bigger」還是往「smaller」猜。
                3. 可以根據他剩下的次數嘲笑他或鼓勵他。
                """
                
                
                response = client.models.generate_content(
                    model='gemini-3.8-flash', 
                    contents=prompt
                )
                
            
                print(f"👉 AI 說：{response.text}")

        except ValueError:
            print("⚠️ Danger：請輸入整數數字，不要亂打字啦！")
    else:
        out_of_limit = True

if out_of_limit:
    print(f"\n💀 次數用盡crying，挑戰失敗！正確答案其實是 {secret_num}")
