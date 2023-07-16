#importing libraries 
from pygame import mixer
from tkinter import *
import tkinter.font as font
from tkinter import filedialog
import vlc



#add many songs to the playlist
def addsongs():
    #a list of songs is returned 
    temp_song=filedialog.askopenfilenames(initialdir="Music/",title="Choose a song", filetypes=(("mp3 Files","*.mp3"),))
    #loop through everyitem in the list
    for s in temp_song:
        s=s.replace("C:/Users/DELL/Music/","")
        sv_list.insert(END,s)

#add many videos to the playlist
def addvideos():
    #a list of videos is returned 
    file=filedialog.askopenfilenames(initialdir="Videos/",title="Choose a video", filetypes=(("mp4 Files","*.mp4"),))
    
    #loop through everyitem in the list
    for i in file:
        i=i.replace("C:/Users/DELL/Videos/Captures/","")
        sv_list.insert(END,i)




def deletesong():
    curr_song=sv_list.curselection()
    sv_list.delete(curr_song[0])


def deletevideo():
    curr_video=sv_list.curselection()
    sv_list.delete(curr_video[0])


def Play():

        global video
        video =sv_list.get(ACTIVE)
        video=f'C:/Users/DELL/Videos/Captures/{video}'
       
        global p
        p = vlc.MediaPlayer(video)

        p.play()
        
    

        song=sv_list.get(ACTIVE)
        song=f'C:/Users/DELL/Music/{song}'
        mixer.music.load(song)
        mixer.music.play()

   
    

#to pause the song 
def Pause():
  
    p.pause()
    mixer.music.pause()

#to stop the  song 
def Stop():
    p.stop()
    mixer.music.stop()
    sv_list.selection_clear(ACTIVE)

#to resume the song

def Resume():
    mixer.music.unpause()

#Function to navigate from the current song
def Previous():
    #to get the selected song index 
    previous_one=sv_list.curselection()
    #to get the previous song index
    previous_one=previous_one[0]-1
    #to get the previous song
    temp2=sv_list.get(previous_one)
    temp2=f'C:/Users/DELL/Music/{temp2}'
    mixer.music.load(temp2)
    mixer.music.play()
    sv_list.selection_clear(0,END)
    #activate new song
    sv_list.activate(previous_one)
    #set the next song
    sv_list.selection_set(previous_one)

def Next():
    #to get the selected song index
    next_one=sv_list.curselection()
    #to get the next song index
    next_one=next_one[0]+1
    #to get the next song 
    temp=sv_list.get(next_one)
    temp=f'C:/Users/DELL/Music/{temp}'
    mixer.music.load(temp)
    mixer.music.play()
    sv_list.selection_clear(0,END)
    #activate newsong
    sv_list.activate(next_one)
     #set the next song
    sv_list.selection_set(next_one)

def Set_vol(val):
    volume=int(val)/100
    mixer.music.set_volume(volume)
    


#creating the root window 
root=Tk()

root.title('Music player')
#initialize mixer 
mixer.init()
# display content in window
Label(root,text="AV PLAYER",font =' ARIAL 20 bold').grid(row=0,column=0)

# image uploading
play_image=PhotoImage(file='play.png')
pause_image=PhotoImage(file='pause.png')
resume_image=PhotoImage(file='resume.png')
next_image=PhotoImage(file='next.png')
previous_image=PhotoImage(file='previous.png')
stop_image=PhotoImage(file='stop.png')


#create the listbox to contain songs
sv_list=Listbox(root,selectmode=SINGLE,bg="light blue",fg="dark blue",font=('arial',12),height=18,width=80,selectbackground="gray",selectforeground="black")
sv_list.grid(columnspan=200)




#play button
play_button=Button(root,text="Play",width =50,command=Play,image=play_image,bg='black',borderwidth=0)

play_button.grid(row=5,column=53)

#pause button 

pause_button=Button(root, text='pause',width=50,command=Pause,image=pause_image,bg='black',borderwidth=0)

pause_button.grid(row=5,column=55)

#stop button
stop_button=Button(root,text="Stop",width =50,command=Stop,image=stop_image,bg='black',borderwidth=0)

stop_button.grid(row=5,column=57)

#resume button
Resume_button=Button(root,text="Resume",width =50,command=Resume,image=resume_image,bg='black',borderwidth=0)

Resume_button.grid(row=5,column=59)

#previous button
previous_button=Button(root,text="Prev",width =50,command=Previous,image=previous_image,bg='black',borderwidth=0)

previous_button.grid(row=5,column=61)

#nextbutton
next_button=Button(root,text="Next",width =50,command=Next,image=next_image,bg='black',borderwidth=0)

next_button.grid(row=5,column=63)

#vol slider
scale=Scale(root, from_=0, to=100,orient=HORIZONTAL,command=Set_vol, length=120)
scale.grid()


#menu 
my_menu=Menu(root)
root.config(menu=my_menu)

add_song_menu=Menu(my_menu)
add_video_menu=Menu(my_menu)
my_menu.add_cascade(label="AUDIO",menu=add_song_menu)
my_menu.add_cascade(label="VIDEO",menu=add_video_menu)
add_song_menu.add_command(label="Add songs",font=('arial',12),command=addsongs)
add_song_menu.add_command(label="Delete song",font=('arial',12),command=deletesong)
add_video_menu.add_command(label="Add videos",font=('arial',12),command=addvideos)
add_video_menu.add_command(label="Delete video",font=('arial',12),command=deletevideo)


mainloop()
