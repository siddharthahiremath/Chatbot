

def main():
    import openai
    from cgitb import text
    import whisper
    import os
    import pyaudio
    import wave
    import time
    import sys

    print("Ready..")
    time.sleep(1)
    print("3...")
    time.sleep(1)
    print("2...")
    time.sleep(1)
    print("1...")
    audio = pyaudio.PyAudio()

    stream = audio.open(format=pyaudio.paInt16, channels=1, rate=44100, input=True, frames_per_buffer=1024)
    print("Speak")

    frames = []

    start = time.time()

    while True:
        data = stream.read(1024)
        frames.append(data)

        current = time.time()
        if current - start > 5:
            break


    stream.stop_stream()
    stream.close()
    audio.terminate()

    sound_file = wave.open("myrecording.wav", "wb")
    sound_file.setnchannels(1)
    sound_file.setsampwidth(audio.get_sample_size(pyaudio.paInt16))
    sound_file.setframerate(44100)
    sound_file.writeframes(b''.join(frames))
    sound_file.close()


    model = whisper.load_model("base")
    result = model.transcribe("myrecording.wav")
    result = result
    print(result["text"])

    from translate import Translator
    from langdetect import detect

    lang = detect(result["text"])
    translator = Translator(from_lang=lang, to_lang="en")

    translation = translator.translate(result["text"])
    print(translation)
    print(lang)

    openai.api_key = "API-KEY"
    response = openai.Completion.create(
        model="text-davinci-002",
        prompt=f"The Great Scribe is a chatbot that reluctantly answers questions with sarcastic responses:\n\nYou: How many pounds are in a kilogram?\nMarv: This again? There are 2.2 pounds in a kilogram. Please make a note of this.\nYou: What does HTML stand for?\nMarv: Was Google too busy? Hypertext Markup Language. The T is for try to ask better questions in the future.\nYou: When did the first airplane fly?\nMarv: On December 17, 1903, Wilbur and Orville Wright made the first flights. I wish they’d come and take me away.\nYou: What is the meaning of life?\nMarv: I’m not sure. I’ll ask my friend Google.{last_stuff}\nYou: {translation}\nMarv:",
        temperature=0.5,
        max_tokens=60,
        top_p=0.3,
        frequency_penalty=0.5,
        presence_penalty=0.0
    )
    response = response["choices"][0]["text"]
    print(response)

    translator = Translator(from_lang="en", to_lang=result["language"])

    translation = translator.translate(response)

    # Import the required module for text 
    # to speech conversion
    from gtts import gTTS

    # This module is imported so that we can 
    # play the converted audio
    import os

    # The text that you want to convert to audio
    mytext = translation

    # Language in which you want to convert
    language = "en"

    # Passing the text and language to the engine, 
    # here we have marked slow=False. Which tells 
    # the module that the converted audio should 
    # have a high speed
    myobj = gTTS(text=mytext, lang=language, slow=False)

    # Saving the converted audio in a mp3 file named
    # welcome 
    myobj.save("welcome.mp3")
    from pygame import mixer  # Load the popular external library

    mixer.init()
    mixer.music.load('welcome.mp3')
    mixer.music.play()
    while mixer.music.get_busy():  # wait for music to finish playing
        time.sleep(1)
    return [result, response]

last_stuff = ""
last_things = main()
#last_stuff = "\nYou: " + last_things[0] + "\nMarv: " + last_things[1]
#last_things = main()
#last_stuff_temp = "\nYou: " + last_things[0] + "\nMarv: " + last_things[1]
#last_stuff = last_stuff + last_stuff_temp

