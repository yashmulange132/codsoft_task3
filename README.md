# codsoft_task3
Sure. Here is a **bigger and more complete Python version** of the recommendation system. It includes user preferences, movie ratings, genre matching, recommendation scores, top recommendations, and an option to add preferences.

```python
users = {
    "Alice": {
        "Action": 5,
        "Comedy": 4,
        "Sci-Fi": 5,
        "Drama": 2,
        "Romance": 1
    },
    "Bob": {
        "Action": 2,
        "Comedy": 5,
        "Sci-Fi": 2,
        "Drama": 5,
        "Romance": 4
    },
    "Charlie": {
        "Action": 5,
        "Comedy": 2,
        "Sci-Fi": 5,
        "Drama": 3,
        "Thriller": 5
    },
    "David": {
        "Action": 2,
        "Comedy": 5,
        "Sci-Fi": 2,
        "Drama": 5,
        "Romance": 5
    }
}

movies = {
    "Inception": {
        "genres": ["Action", "Sci-Fi", "Thriller"],
        "rating": 8.8,
        "year": 2010
    },
    "Avengers": {
        "genres": ["Action", "Sci-Fi"],
        "rating": 8.0,
        "year": 2012
    },
    "The Hangover": {
        "genres": ["Comedy"],
        "rating": 7.7,
        "year": 2009
    },
    "Titanic": {
        "genres": ["Romance", "Drama"],
        "rating": 7.9,
        "year": 1997
    },
    "Interstellar": {
        "genres": ["Sci-Fi", "Drama"],
        "rating": 8.7,
        "year": 2014
    },
    "The Notebook": {
        "genres": ["Romance", "Drama"],
        "rating": 7.8,
        "year": 2004
    },
    "Joker": {
        "genres": ["Drama", "Thriller"],
        "rating": 8.4,
        "year": 2019
    },
    "Guardians of the Galaxy": {
        "genres": ["Action", "Comedy", "Sci-Fi"],
        "rating": 8.0,
        "year": 2014
    },
    "Iron Man": {
        "genres": ["Action", "Sci-Fi"],
        "rating": 7.9,
        "year": 2008
    },
    "The Dark Knight": {
        "genres": ["Action", "Drama", "Thriller"],
        "rating": 9.0,
        "year": 2008
    },
    "Spider-Man": {
        "genres": ["Action", "Adventure", "Sci-Fi"],
        "rating": 8.2,
        "year": 2002
    },
    "Jurassic Park": {
        "genres": ["Action", "Adventure", "Sci-Fi"],
        "rating": 8.2,
        "year": 1993
    },
    "Toy Story": {
        "genres": ["Comedy", "Adventure"],
        "rating": 8.3,
        "year": 1995
    },
    "Frozen": {
        "genres": ["Comedy", "Drama", "Romance"],
        "rating": 7.4,
        "year": 2013
    },
    "The Matrix": {
        "genres": ["Action", "Sci-Fi", "Thriller"],
        "rating": 8.7,
        "year": 1999
    }
}

def calculate_score(preferences, movie_data):
    score = 0

    for genre in movie_data["genres"]:
        if genre in preferences:
            score += preferences[genre]

    rating_score = movie_data["rating"] / 2

    final_score = score + rating_score

    return final_score


def recommend_movies(user):
    preferences = users[user]
    recommendations = []

    for movie, movie_data in movies.items():
        score = calculate_score(preferences, movie_data)

        recommendations.append({
            "movie": movie,
            "score": score,
            "rating": movie_data["rating"],
            "year": movie_data["year"],
            "genres": movie_data["genres"]
        })

    recommendations.sort(
        key=lambda x: x["score"],
        reverse=True
    )

    return recommendations


def display_recommendations(user):
    recommendations = recommend_movies(user)

    print("\nRecommended Movies for", user)
    print("=" * 60)

    for index, movie in enumerate(recommendations[:10], 1):
        print(
            f"{index}. {movie['movie']} "
            f"({movie['year']})"
        )
        print(
            f"   Genres: {', '.join(movie['genres'])}"
        )
        print(
            f"   Movie Rating: {movie['rating']}/10"
        )
        print(
            f"   Recommendation Score: "
            f"{movie['score']:.2f}"
        )
        print("-" * 60)


def display_user_preferences(user):
    print("\nPreferences of", user)
    print("=" * 40)

    preferences = users[user]

    for genre, value in preferences.items():
        print(f"{genre}: {value}/5")


def display_all_movies():
    print("\nAvailable Movies")
    print("=" * 60)

    for index, (movie, data) in enumerate(movies.items(), 1):
        print(f"{index}. {movie}")
        print(f"   Year: {data['year']}")
        print(f"   Rating: {data['rating']}/10")
        print(f"   Genres: {', '.join(data['genres'])}")
        print("-" * 60)


def add_preference(user):
    print("\nAvailable Genres:")
    print("Action")
    print("Comedy")
    print("Sci-Fi")
    print("Drama")
    print("Romance")
    print("Thriller")
    print("Adventure")

    genre = input("\nEnter genre: ").strip()
    
    try:
        rating = int(input("Enter preference rating from 1 to 5: "))

        if rating < 1 or rating > 5:
            print("Rating must be between 1 and 5.")
            return

        users[user][genre] = rating

        print("Preference added successfully.")

    except ValueError:
        print("Please enter a valid number.")


def search_movie():
    movie_name = input("\nEnter movie name: ").strip().lower()

    found = False

    for movie, data in movies.items():
        if movie.lower() == movie_name:
            print("\nMovie Found")
            print("=" * 40)
            print("Name:", movie)
            print("Year:", data["year"])
            print("Rating:", data["rating"])
            print("Genres:", ", ".join(data["genres"]))
            found = True
            break

    if not found:
        print("Movie not found.")


def add_user():
    name = input("\nEnter new user name: ").strip()

    if name in users:
        print("User already exists.")
        return

    preferences = {}

    print("\nEnter your preferred genres.")
    print("Enter 'done' when finished.")

    while True:
        genre = input("Genre: ").strip()

        if genre.lower() == "done":
            break

        try:
            rating = int(
                input("Preference rating from 1 to 5: ")
            )

            if rating < 1 or rating > 5:
                print("Rating must be between 1 and 5.")
                continue

            preferences[genre] = rating

        except ValueError:
            print("Enter a valid rating.")

    users[name] = preferences

    print("New user added successfully.")


def show_menu():
    print("\n")
    print("=" * 60)
    print("              MOVIE RECOMMENDATION SYSTEM")
    print("=" * 60)
    print("1. Get Movie Recommendations")
    print("2. View User Preferences")
    print("3. View All Movies")
    print("4. Search for a Movie")
    print("5. Add Genre Preference")
    print("6. Add New User")
    print("7. Exit")
    print("=" * 60)


def select_user():
    print("\nAvailable Users:")

    for index, user in enumerate(users.keys(), 1):
        print(f"{index}. {user}")

    user = input("\nEnter user name: ").strip()

    if user in users:
        return user

    print("User not found.")
    return None


def main():
    print("=" * 60)
    print("       WELCOME TO THE MOVIE RECOMMENDATION SYSTEM")
    print("=" * 60)

    while True:
        show_menu()

        choice = input("Enter your choice: ").strip()

        if choice == "1":
            user = select_user()

            if user:
                display_recommendations(user)

        elif choice == "2":
            user = select_user()

            if user:
                display_user_preferences(user)

        elif choice == "3":
            display_all_movies()

        elif choice == "4":
            search_movie()

        elif choice == "5":
            user = select_user()

            if user:
                add_preference(user)

        elif choice == "6":
            add_user()

        elif choice == "7":
            print("\nThank you for using the Recommendation System.")
            break

        else:
            print("\nInvalid choice. Please try again.")


if __name__ == "__main__":
    main()
```
