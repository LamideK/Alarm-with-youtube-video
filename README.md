# Alarm-with-youtube-video
this calls up a random video from a youtube link as an alarm
# This works with a youtube link such that when the user sets the alarm, it picks up a random link from that directory
    import datetime
    import os
    import time
    import random
    import webbrowser
    from pip._vendor.distlib.compat import raw_input

# create a file path if it doesnt already exist
    if not os.path.isfile("youtube_alarm_videos.txt"):
        print('Creating "youtube_alarm_videos.txt"...')
        with open("youtube_alarm_videos.txt", "w") as alarm_file:
            alarm_file.write("https://www.youtube.com/watch?v=anM6uIZvx74")

# function to check alarm time is valid
    def check_alarm_time(alarm_time):
        if len(alarm_time) == 1:
            if 24 > alarm_time[0] >= 0:
                return True
        # [Hour: Minute] Format
        if len(alarm_time) == 2:
            if 24 > alarm_time[0] >= 0 and \
                    60 > alarm_time[1] >= 0:
                return True
        # [Hour: Minute:Second] Format
        elif len(alarm_time) == 3:
            if 24 > alarm_time[0] >= 0 and \
                  60 > alarm_time[1] >= 0 and 60 > alarm_time[2] >= 0:
                return True
        return False

# Get user input
    print("Set a time for the alarm (Ex. 06:30 or 18:30:00)")
    while True:
        alarm_input = raw_input('Here: ')
        try:
            alarm_time = [int(n) for n in alarm_input.split(":")]
            if check_alarm_time(alarm_time):
                break
            else:
                raise ValueError
        except ValueError:
            print('ERROR')

# Convert the time to total number of seconds
    seconds = [3600,60,1] #hours, minutes and seconds
    alarm_seconds = sum([a*b for a,b in zip(seconds[:len(alarm_time)], alarm_time)])

# Get current time of day in seconds
    now = datetime.datetime.now()
    current_time_seconds = sum([a*b for a,b in zip(seconds, [now.hour, now.minute, now.second])])

# Calculate the number of seconds until alarm goes off (If time difference is negative, set alarm for next day)
    time_diff_seconds = alarm_seconds - current_time_seconds
    if time_diff_seconds < 0:
        time_diff_seconds += 86400 # number of seconds in a day

# Display the amount of time until the alarm goes off
    print("Alarm set to go off in %s" % datetime.timedelta(seconds=time_diff_seconds))
    time.sleep(time_diff_seconds) # Sleep until the alarm goes off
# Time for the alarm to go off
    print("Wake Up!")
    with open("youtube_alarm_videos.txt", "r") as alarm_file:
        videos = alarm_file.readlines()
    webbrowser.open(random.choice(videos)) # Opens the file, and uses a random video choice
