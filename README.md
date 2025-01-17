# Gen-AI-Space-project
import openai
from textblob import TextBlob

openai.api_key = "api_key"#here api key is not accepting to paste here.

conversation_history = []

def analyze_sentiment(text):
    sentiment = TextBlob(text).sentiment.polarity
    if sentiment > 0:
        return "Positive 😊"
    elif sentiment < 0:
        return "Negative 😟"
    else:
        return "Neutral 😐"

def generate_response(prompt, personality):
    try:
        if personality.lower() == "friendly":
            prompt = f"You are a friendly chatbot. {prompt}"
        elif personality.lower() == "humorous":
            prompt = f"You are a humorous chatbot. {prompt}"
        else:
            prompt = f"You are a formal chatbot. {prompt}"
        
        response = openai.Completion.create(
            engine="text-davinci-003",
            prompt=prompt,
            max_tokens=200,
            temperature=0.7
        )
        return response['choices'][0]['text'].strip()
    except Exception as e:
        return f"Error: {str(e)}"

def main():
    print("Welcome to the Enhanced Gen AI Chatbot!")
    print("Choose a personality for the chatbot: (1) Friendly (2) Formal (3) Humorous")
    personality_choice = input("Enter your choice (1/2/3): ")
    personality = "Friendly" if personality_choice == "1" else "Formal" if personality_choice == "2" else "Humorous"
    print(f"\nChatbot personality set to: {personality}")
    print("Type 'suggest' if you want topic suggestions or 'exit' to quit.\n")

    while True:
        user_input = input("\nYou: ")
        if user_input.lower() == "exit":
            print("Chatbot: Goodbye! Have a great day!")
            break
        elif user_input.lower() == "suggest":
            print("Chatbot: You can talk about technology, movies, travel, or hobbies. What interests you?")
            continue
        
        sentiment = analyze_sentiment(user_input)
        print(f"Chatbot: I detected your sentiment as: {sentiment}")
        
        conversation_history.append(f"You: {user_input}")
        response = generate_response(" ".join(conversation_history), personality)
        conversation_history.append(f"Chatbot: {response}")
        print(f"Chatbot: {response}")

if __name__ == "__main__":
    main()
