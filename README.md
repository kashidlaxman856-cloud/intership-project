# AI CHATBOT FOR COLLEGE HELPDESK

CODE PERFORMED :
import re
import random

class CollegeChatbot:
    def __init__(self):
        # Define intents, keywords, and matching responses
        self.rules = {
            'greeting': {
                'keywords': [r'\bhello\b', r'\bhi\b', r'\bhey\b', r'\bsup\b', r'\bgreetings\b'],
                'responses': ["Hello! Welcome to the College Helpdesk. How can I assist you today?", 
                              "Hi there! Ask me anything about admissions, exams, or campus facilities.",
                              "Welcome! What information can I help you with?"]
            },
            'admissions': {
                'keywords': [r'\badmission\b', r'\badmissions\b', r'\bapply\b', r'\bregistration\b', r'\bfees\b', r'\benrollment\b'],
                'responses': [
                    "Admissions for the upcoming semester are open! You can apply online via the college portal. The application fee is $50, and the deadline is next month.",
                    "To apply for admission, visit our official website and fill out the online form. You'll need your academic transcripts and entrance exam scores.",
                    "The admission process is simple: Submit your application, pay the fee, and wait for the merit list. Need any specific details?"
                ]
            },
            'exams': {
                'keywords': [r'\bexam\b', r'\bexams\b', r'\btest\b', r'\bschedule\b', r'\btimetable\b', r'\bmidterm\b', r'\bfinal\b'],
                'responses': [
                    "The mid-term exam schedule has been posted on the student notice board. Final exams are scheduled to begin from the first week of next month. Make sure your dues are cleared!",
                    "Exam schedules are available on the student portal. Remember to carry your hall ticket and valid ID during exams.",
                    "For exam-related queries, you can also visit the Academic Office. They're open from 9 AM to 5 PM on weekdays."
                ]
            },
            'facilities': {
                'keywords': [r'\blibrary\b', r'\bcampus\b', r'\bhostel\b', r'\bgym\b', r'\bwi-fi\b', r'\bwifi\b', r'\bfacilities\b', r'\blab\b'],
                'responses': [
                    "Our campus features a 24/7 library, fully equipped computer labs, high-speed Wi-Fi, separate hostels for boys and girls, and a modern sports complex.",
                    "Campus facilities include: Central Library (24/7 access), Computer Labs, Cafeteria, Gymnasium, Sports Court, and High-Speed Internet throughout campus.",
                    "All students have access to our world-class facilities. Hostel accommodation is available for both male and female students."
                ]
            },
            'contact': {
                'keywords': [r'\bcontact\b', r'\bphone\b', r'\bemail\b', r'\baddress\b', r'\boffice\b'],
                'responses': [
                    "You can reach us at:\n📧 Email: administration@college.edu\n📞 Phone: +1-800-COLLEGE\n📍 Address: 123 Education Lane, College City",
                    "Contact Information:\n- Main Office: +1-800-COLLEGE\n- Email: info@college.edu\n- Admissions: admissions@college.edu"
                ]
            },
            'goodbye': {
                'keywords': [r'\bbye\b', r'\bgoodbye\b', r'\bexit\b', r'\bthank you\b', r'\bthanks\b', r'\bquit\b'],
                'responses': [
                    "You're welcome! Feel free to reach out if you have more questions. Have a great day!",
                    "Goodbye! Glad I could help. Good luck with your studies!",
                    "Thank you for using College Helpdesk. See you soon!"
                ]
            }
        }
        self.fallback_responses = [
            "I'm sorry, I didn't quite catch that. Could you please rephrase your question regarding admissions, exams, or campus facilities?",
            "I am still learning! For specific queries like that, please contact the main office at administration@college.edu.",
            "That's an interesting question! Could you provide more details? I can help with admissions, exams, facilities, or contact information."
        ]
        self.conversation_history = []

    def classify_intent(self, user_message):
        """Classify user intent based on keywords"""
        user_message = user_message.lower().strip()
        
        # Handle empty input
        if not user_message:
            return 'unknown'
        
        # Loop through intents to find a keyword match (Intent Classification)
        for intent, data in self.rules.items():
            for pattern in data['keywords']:
                if re.search(pattern, user_message):
                    return intent
        return 'unknown'

    def get_response(self, intent):
        """Get a random response for the classified intent"""
        if intent in self.rules:
            return random.choice(self.rules[intent]['responses'])
        return random.choice(self.fallback_responses)

    def save_conversation(self, user_msg, bot_response):
        """Save conversation for future reference"""
        self.conversation_history.append({
            'user': user_msg,
            'bot': bot_response
        })

    def display_help(self):
        """Display available commands and topics"""
        print("\n" + "="*50)
        print("📚 AVAILABLE TOPICS:")
        print("="*50)
        print("✓ Admissions & Registration")
        print("✓ Exams & Schedules")
        print("✓ Campus Facilities")
        print("✓ Contact Information")
        print("✓ Type 'help' for this menu")
        print("✓ Type 'history' to see conversation log")
        print("✓ Type 'exit' or 'bye' to quit")
        print("="*50 + "\n")

    def start_chat(self):
        """Start the chatbot conversation"""
        print("\n" + "="*60)
        print("🎓 COLLEGE HELPDESK AI CHATBOT ACTIVE 🎓")
        print("="*60)
        print("Welcome! I'm here to help with college information.")
        print("Type 'help' for available topics or 'exit' to quit.")
        print("="*60)
        
        while True:
            try:
                user_input = input("\n📝 You: ").strip()
                
                # Handle empty input
                if not user_input:
                    print("Bot: Please enter a question or type 'help' for available topics.")
                    continue
                
                # Handle special commands
                if user_input.lower() == 'help':
                    self.display_help()
                    continue
                
                if user_input.lower() == 'history':
                    if self.conversation_history:
                        print("\n📜 CONVERSATION HISTORY:")
                        for i, msg in enumerate(self.conversation_history, 1):
                            print(f"{i}. You: {msg['user']}")
                            print(f"   Bot: {msg['bot']}\n")
                    else:
                        print("Bot: No conversation history yet.")
                    continue
                
                # Classify intent and get response
                intent = self.classify_intent(user_input)
                response = self.get_response(intent)
                
                print(f"🤖 Bot: {response}")
                
                # Save to conversation history
                self.save_conversation(user_input, response)
                
                # Check if user wants to exit
                if intent == 'goodbye':
                    print("\n" + "="*60)
                    print("Thank you for using College Helpdesk!")
                    print("="*60)
                    break
                    
            except KeyboardInterrupt:
                print("\n\nBot: Chat ended. Goodbye!")
                break
            except Exception as e:
                print(f"Bot: An error occurred: {str(e)}. Please try again.")

if __name__ == "__main__":
    chatbot = CollegeChatbot()
    chatbot.start_chat()

https://github.com/user-attachments/assets/c0897ab4-f32f-4886-9314-8668f9c1612d

