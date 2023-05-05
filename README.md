import speech_recognition as sr
import serial

# Set up serial communication with the robot
ser = serial.Serial('/dev/ttyACM0', 9600) # Replace with your serial port and baud rate

# Initialize the speech recognition engine
r = sr.Recognizer()

# Set up the microphone as the source for the speech recognition
with sr.Microphone() as source:
    print("Say something!")
    while True:
        # Listen for audio input
        audio = r.listen(source)

        try:
            # Recognize speech using Google Speech Recognition
            command = r.recognize_google(audio)
            print("You said: " + command)

            # Send the command to the robot
            ser.write(command.encode())

        except sr.UnknownValueError:
            print("Google Speech Recognition could not understand audio")
        except sr.RequestError as e:
            print("Could not request results from Google Speech Recognition service; {0}".format(e))
