# MUSIC-RECOMMENDATION

MUSIC RECOMMENDATION ACCORDING TO VIBE IN PYTHON

import pandas as pd
import math
import random

# 1. Expanded Dataset with multi-language mix
data = {
    'song': ['Tum Hi Ho', 'Raabta', 'Chitta Kukkad', 'Ghoomar', 'Butta Bomma', 
             'Kolaveri Di', 'Shape of You', 'Ami Je Tomar', 'Gallan Goodiyaan', 
             'Rowdy Baby', 'Saami Saami', 'Kun Faya Kun', 'Hawayein', 'Pasoori', 
             'Natu Natu', 'Kesariya', 'Samjhawan', 'Maahi Ve', 'Zara Zara', 
             'Dhoom Machale', 'Pee Loon', 'Sunn Raha Hai', 'Apna Bana Le'],
    'artist': ['Arijit Singh', 'Arijit Singh', 'Punjabi Folk', 'Rajasthani Folk', 'Armaan Malik', 
               'Dhanush', 'Ed Sheeran', 'Shreya Ghoshal', 'Sukhwinder Singh', 
               'Dhanush', 'Sunidhi Chauhan', 'A.R. Rahman', 'Arijit Singh', 'Ali Sethi', 
               'M.M. Keeravani', 'Arijit Singh', 'Arijit Singh', 'KK', 'Bombay Jayashri', 
               'Sunidhi Chauhan', 'Mohit Chauhan', 'Ankit Tiwari', 'Arijit Singh'],
    'language': ['Hindi', 'Hindi', 'Punjabi', 'Rajasthani', 'Telugu', 
                 'Tamil', 'English', 'Bengali', 'Hindi', 
                 'Tamil', 'Telugu', 'Hindi', 'Hindi', 'Punjabi', 
                 'Telugu', 'Hindi', 'Hindi', 'Hindi', 'Hindi', 
                 'Hindi', 'Hindi', 'Hindi', 'Hindi'],
    'valence': [0.4, 0.6, 0.7, 0.6, 0.8, 0.5, 0.9, 0.3, 0.9, 0.8, 0.7, 0.2, 0.5, 0.6, 0.9, 0.4, 0.3, 0.6, 0.3, 0.8, 0.5, 0.3, 0.5],
    'energy': [0.3, 0.7, 0.8, 0.5, 0.9, 0.6, 0.8, 0.2, 0.9, 0.9, 0.8, 0.2, 0.4, 0.7, 0.9, 0.3, 0.3, 0.7, 0.2, 0.9, 0.6, 0.5, 0.4]
}

df = pd.DataFrame(data)

def recommend_by_vibe(target_v, target_e):
    # Calculate distance for all songs
    df_calc = df.copy()
    df_calc['distance'] = df_calc.apply(
        lambda row: math.sqrt((row['valence'] - target_v)**2 + (row['energy'] - target_e)**2), axis=1
    )
    
    # Get the top 3 closest matches
    top_matches = df_calc.nsmallest(3, 'distance')
    
    # Pick one randomly from those top 3
    suggestion = top_matches.sample(n=1).iloc[0]
    return f"{suggestion['song']} by {suggestion['artist']} ({suggestion['language']})"

# 2. Dynamic Mapping
mood_map = {
    "happy": (0.9, 0.9), "energetic": (0.8, 0.9), "sad": (0.2, 0.2),
    "calm": (0.4, 0.3), "romantic": (0.6, 0.4), "party": (0.9, 0.8)
}

print("How are you feeling? (happy, energetic, sad, calm, romantic, party)")
user_mood = input("Enter mood: ").lower()

if user_mood in mood_map:
    v, e = mood_map[user_mood]
    print(f"\nWe recommend: {recommend_by_vibe(v, e)}")
else:
    print("Vibe not found. Try 'happy', 'party', or 'calm'.")
