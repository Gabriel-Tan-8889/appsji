import time

class StudyApp:
    def __init__(self):
        self.accounts = {}  # username: password
        self.failed_attempts = {}
        self.locks = {}
        self.current_user = None
        self.points = {}
        self.friends = {}
        self.flashcards = {}
        self.daily_challenges = {}
        self.calendar = {}
        self.rewards = {}
        self.past_usernames = []
        self.friend_requests = {}  # username: list of usernames who requested

    def ensure_user(self, username):
        if username not in self.points:
            self.points[username] = 0
        if username not in self.friends:
            self.friends[username] = []
        if username not in self.flashcards:
            self.flashcards[username] = []
        if username not in self.daily_challenges:
            self.daily_challenges[username] = []
        if username not in self.calendar:
            self.calendar[username] = {}
        if username not in self.rewards:
            self.rewards[username] = []
        if username not in self.failed_attempts:
            self.failed_attempts[username] = 0
        if username not in self.locks:
            self.locks[username] = 0
        if username not in self.friend_requests:
            self.friend_requests[username] = []

    def start_screen(self):
        print("Welcome to Study App!")
        while True:
            self.login_flow()

    def login_flow(self):
        if self.past_usernames:
            print("\nPreviously used usernames:")
            for i, uname in enumerate(self.past_usernames, 1):
                print(f"{i}. {uname}")
        username = input("Enter username: ")
        if username not in self.past_usernames:
            self.past_usernames.append(username)
        self.ensure_user(username)
        if self.locks[username] > 0:
            print(f"Account locked. Please wait {self.locks[username]} seconds.")
            time.sleep(self.locks[username])
            self.locks[username] = 0
        password = input("Enter password: ")
        if self.check_credentials(username, password):
            self.current_user = username
            print("Login successful!")
            self.check_friend_requests(username)
            self.loading_screen()
        else:
            print("Incorrect credentials.")
            self.failed_attempts[username] += 1
            if input("Forgot password? (y/n): ").lower() == "y":
                self.reset_password(username)
            elif username not in self.accounts:
                if input("No account! Sign up? (y/n): ").lower() == "y":
                    self.sign_up(username)
                return
            elif self.failed_attempts[username] >= 3:
                print("Locked out for 5 seconds.")
                self.locks[username] = 5
                self.failed_attempts[username] = 0
                time.sleep(5)
                self.locks[username] = 0
            return

    def check_friend_requests(self, username):
        pending = self.friend_requests[username]
        if pending:
            print("\nYou have friend requests from:")
            for requester in pending:
                print(f"- {requester}")
            to_accept = input("Enter usernames to accept (comma-separated, or blank to ignore): ")
            accepted = [u.strip() for u in to_accept.split(',') if u.strip()]
            for req in accepted:
                if req in pending:
                    if req not in self.friends[username]:
                        self.friends[username].append(req)
                    if username not in self.friends[req]:
                        self.friends[req].append(username)
                    print(f"Accepted friend request from {req}.")
                    pending.remove(req)
            # Remove any accepted from requests
            self.friend_requests[username] = [req for req in pending if req not in accepted]

    def check_credentials(self, username, password):
        return self.accounts.get(username) == password

    def reset_password(self, username):
        if username not in self.accounts:
            print("No account detected.")
            return
        new_password = input("Enter new password: ")
        self.accounts[username] = new_password
        self.failed_attempts[username] = 0
        print("Password reset. Please login again.")

    def sign_up(self, username):
        if username in self.accounts:
            print("Username already exists.")
            return
        password = input("Enter new password: ")
        confirm = input("Confirm password: ")
        if password != confirm:
            print("Passwords do not match.")
            return
        self.accounts[username] = password
        self.ensure_user(username)
        print("Account created! Please login.")

    def loading_screen(self):
        print("Loading into app...")
        self.menu()

    def menu(self):
        while True:
            print("\nWhat screen to go to?")
            print("1. Study Timer\n2. Flashcards\n3. Data Planner\n4. Daily Challenges\n5. End")
            choice = input("Choice: ")
            if choice == "1":
                self.study_timer()
            elif choice == "2":
                self.flashcards_menu()
            elif choice == "3":
                self.data_planner()
            elif choice == "4":
                self.daily_challenges_menu()
            elif choice == "5":
                print("Goodbye!")
                break
            else:
                print("Invalid choice.")

    def study_timer(self):
        print("Study Timer")
        input("Press Enter to lock phone and start studying...")
        print("Phone locked. Timer started. (Simulating study for 1 second)")
        time.sleep(1)
        input("Press Enter to stop studying and unlock phone...")
        print("Phone unlocked. You studied for 1 hour.")
        self.points[self.current_user] += 2
        print("You earned 2 points!")
        self.leaderboard()

    def flashcards_menu(self):
        while True:
            print("\nFlashcards Menu")
            print("1. Create Flashcard\n2. Study Flashcards\n3. Back")
            choice = input("Choice: ")
            if choice == "1":
                self.create_flashcard()
            elif choice == "2":
                self.study_flashcards()
            elif choice == "3":
                break
            else:
                print("Invalid choice.")

    def create_flashcard(self):
        question = input("Enter question: ")
        answer = input("Enter answer: ")
        self.flashcards[self.current_user].append((question, answer))
        print("Flashcard created.")

    def study_flashcards(self):
        cards = self.flashcards[self.current_user]
        if not cards:
            print("No flashcards to study.")
            return
        for q, a in cards:
            print(f"Q: {q}")
            input("Press Enter to show answer...")
            print(f"A: {a}")
            level = input("Select difficulty (easy/medium/hard): ").lower()
            points = {"easy": 1, "medium": 2, "hard": 3}.get(level, 1)
            self.points[self.current_user] += points
            print(f"You earned {points} points!")
        self.leaderboard()

    def data_planner(self):
        print("\nData Planner")
        date = input("Enter date to plan (YYYY-MM-DD): ")
        subject = input("Enter subject/task: ")
        self.calendar[self.current_user][date] = subject
        print(f"Planned '{subject}' for {date}.")
        today = input("Is this for today? (y/n): ").lower() == "y"
        if today:
            print(f"Reminder: Study '{subject}' today!")
            self.points[self.current_user] += 1
        self.leaderboard()

    def daily_challenges_menu(self):
        while True:
            print("\nDaily Challenges Menu")
            print("1. Generate New Challenge\n2. Answer Challenge\n3. Back")
            choice = input("Choice: ")
            if choice == "1":
                self.generate_challenge()
            elif choice == "2":
                self.answer_challenge()
            elif choice == "3":
                break
            else:
                print("Invalid choice.")

    def generate_challenge(self):
        q = input("Enter challenge question: ")
        a = input("Enter challenge answer: ")
        self.daily_challenges[self.current_user].append({"question": q, "answer": a, "attempted": False})
        print("Challenge generated.")

    def answer_challenge(self):
        challenges = [c for c in self.daily_challenges[self.current_user] if not c.get("attempted", False)]
        if not challenges:
            print("No available challenges.")
            return
        for c in challenges:
            print(f"Challenge: {c['question']}")
            answer = input("Your answer: ")
            if answer.strip().lower() == c["answer"].strip().lower():
                print("Correct!")
                self.points[self.current_user] += 1
                c["attempted"] = True
                print("You earned 1 point!")
            else:
                print("Incorrect. Please try again later.")
        self.leaderboard()

    def leaderboard(self):
        print(f"\nLeaderboard - Your points: {self.points[self.current_user]}")
        # Show friends and their points, sorted by points descending
        friends_and_you = [self.current_user] + self.friends[self.current_user]
        leaderboard_list = [(uname, self.points.get(uname, 0)) for uname in friends_and_you]
        leaderboard_list = list({uname: pts for uname, pts in leaderboard_list}.items())  # remove duplicates
        leaderboard_list.sort(key=lambda tup: tup[1], reverse=True)
        print("Leaderboard (You and Friends):")
        for rank, (uname, pts) in enumerate(leaderboard_list, 1):
            you_mark = " (You)" if uname == self.current_user else ""
            print(f"{rank}. {uname}{you_mark}: {pts} points")
        if input("Add friend via code? (y/n): ").lower() == "y":
            friend = input("Enter friend's username: ")
            if friend in self.accounts and friend != self.current_user:
                if self.current_user not in self.friend_requests[friend] and self.current_user not in self.friends[friend]:
                    self.friend_requests[friend].append(self.current_user)
                    print(f"Friend request sent to {friend}.")
                elif self.current_user in self.friends[friend]:
                    print("You are already friends!")
                else:
                    print("Friend request already sent.")
            else:
                print("No such user.")
        if self.points[self.current_user] >= 10:
            self.show_rewards()

    def show_rewards(self):
        print("\nRewards available! Choose one:")
        print("1. Gift card\n2. Game prop\n3. Extra theme\n4. Pet\n5. Extra content\n6. Keep my points (no reward)")
        choice = input("Your reward: ")
        rewards_map = {
            "1": "Gift card",
            "2": "Game prop",
            "3": "Extra theme",
            "4": "Pet",
            "5": "Extra content",
            "6": "Keep points"
        }
        reward = rewards_map.get(choice, "Gift card")
        if choice == "6":
            print("You chose to keep your points. No points deducted and no reward received.")
        else:
            self.rewards[self.current_user].append(reward)
            self.points[self.current_user] -= 10
            print(f"You redeemed: {reward} (10 points used).")

if __name__ == "__main__":
    app = StudyApp()
    app.start_screen()
