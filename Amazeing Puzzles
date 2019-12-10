import random
import time
import pygame
import math
from pygame.locals import *
FPS = 30 # frames per second to update the screen
winWidth = 800 # width of the program's window, in pixels
winHeight = 600 # height in pixels
midWidth = int(winWidth/2)
midHeight = int(winHeight/2)
spaceWidth = 15
pygame.init()
FPSClock = pygame.time.Clock()
dispSurf = pygame.display.set_mode((winWidth, winHeight))
rawLevels = open("Amazeing Puzzles Mazes.txt", "r+").read()
levels = rawLevels.split(":")
rawPuzzles = open("Amazeing Puzzles Logic Puzzles.txt", "r+").read()
puzzles = rawPuzzles.split(":")
spawnX = (0, 253, 215, 178)
spawnY = (0, 363, 408, 438)
drawLeftWall = ("3", "7", "9", "c", "d", "e", "g", "1", "h")
drawTopWall = ("4", "8", "9", "a", "d", "f", "g", "1")
drawRightWall = ("5", "7", "a", "b", "d", "e", "f", "1", "h")
drawBottomWall = ("6", "8", "b", "c", "e", "f", "g", "h")
white = (255, 255, 255)
hide = (0, 0, 0)
end = False
clueTextShown = False
pygame.display.set_caption('Amazeing Puzzles')
#pygame.draw.line(dispSurf, white, (200, 200), (300, 300), 1)
#pygame.draw.lines(dispSurf, white, True, [(10, 10), (790, 10), (790, 590), (10, 590)]) #og test lines
player = pygame.draw.rect(dispSurf, white, (103, 100, 10, 10))
playerLocation = 0
newClueFont = pygame.font.Font(None, 40)
newClueText = newClueFont.render("A new clue has appered!", 1, white, dispSurf)
def teleport(x, y):
    global player
    x = x-player.x
    y = y-player.y
    pygame.draw.rect(dispSurf, hide, player)
    player = player.move(x, y)
    pygame.draw.rect(dispSurf, white, player)
 
def move(direction):
    global player, level, width, playerLocation, clueLocations, clues, clueTextShown
    tileOn = level[playerLocation]
    pygame.draw.rect(dispSurf, hide, player)
    if (direction == "up" and tileOn not in drawTopWall):
        player = player.move(0, -spaceWidth)
        playerLocation -= width+1
    elif (direction == "down" and tileOn not in drawBottomWall):
        player = player.move(0, spaceWidth)
        playerLocation += width+1
    elif (direction == "left" and tileOn not in drawLeftWall):
        player = player.move(-spaceWidth, 0)
        playerLocation -= 1
    elif (direction == "right" and tileOn not in drawRightWall):
        player = player.move(spaceWidth, 0)
        playerLocation += 1
    pygame.draw.rect(dispSurf, white, player)
    if (clueTextShown):
        pygame.draw.rect(dispSurf, hide, (250, 500, 350, 100))
    if (playerLocation in clueLocations):
        where = clueLocations.index(playerLocation)
        print(clues[where])
        del clues[where]
        del clueLocations[where]
        print("There are "+str(len(clueLocations)-1)+" clues left")
        dispSurf.blit(newClueText, (250, 500))
        clueTextShown = True
    elif (level[playerLocation] == "1"):
        print("You reached the end!")
        out = checkAnswers()
        return out
 
def setupLogicPuzzle():
    global player, playerLocation, level, puzzles, clues, answers, numOfAnswers
    puzzle = puzzles[levelNum*2]
    puzzle = puzzle.split("-")
    numOfAnswers = puzzle[1][0]
    puzzleSelected = random.randint(1, int(puzzle[0]))
    answers = puzzle[int(puzzle[0])*2+puzzleSelected*2]
    puzzle = puzzle[puzzleSelected*2]
    clues = puzzle.split("\n")
    answers = answers.split("\n")
    del clues[0]
    del clues[len(clues)-1]
    del answers[0]
    del answers[len(answers)-1]
    print(clues[0])
 
def drawLevel(levelNum, startX, endX, startY, endY, level):
    #The first char is a enter, start at 1 when reading the level.
    z = 1
    for y in range(startY, endY, spaceWidth):
        for x in range(startX, endX, spaceWidth):
            spot = level[z]
            if (not(spot == "0" or spot == "2" or spot == "/n")):#Has Walls
                if (spot in drawLeftWall):
                    pygame.draw.line(dispSurf, white, (x,y), (x,y+spaceWidth), 1)
                if (spot in drawTopWall):
                    pygame.draw.line(dispSurf, white, (x,y), (x+spaceWidth,y), 1)
                if (spot in drawRightWall):
                    pygame.draw.line(dispSurf, white, (x+spaceWidth,y), (x+spaceWidth,y+spaceWidth), 1)
                if (spot in drawBottomWall):
                    pygame.draw.line(dispSurf, white, (x,y+spaceWidth), (x+spaceWidth,y+spaceWidth), 1)
            z = z+1
        z = z+1
    pygame.display.update()
 
def setupLevel(levelIn, levels, player):
    global level, width, height, playerLocation, levelNum
    levelNum = levelIn
    level = levels[levelNum*2]
    level = level.split("w")
    width = int(level[0])
    height = int(level[1])
    level = level[2]
    startX = math.ceil(midWidth-((width/2)*spaceWidth))
    endX = math.ceil(midWidth+((width/2)*spaceWidth))
    startY = math.ceil(midHeight-((height/2)*spaceWidth))
    endY = math.ceil(midHeight+((height/2)*spaceWidth))
    drawLevel(levelNum, startX, endX, startY, endY, level)
    playerLocation = level.index("h")
    x = spawnX[levelNum]
    y = spawnY[levelNum]
    teleport(x, y)
 
def hideClues():
    global clues, answers, level, clueLocations
    clueLocations = [-1]
    for clueNum in range(1, len(clues)):
        spot = level.index("h")
        while (level[spot] == "h" or level[spot] == "1" or level[spot] == "0" or (level[spot] == "\n") or (spot in clueLocations)):
            spot = random.randint(0, len(level)-1)
        clueLocations.append(spot)
 
def homeScreen():
    global end
    pygame.draw.rect(dispSurf, hide, (0, 0, winWidth, winHeight))
    titleFont = pygame.font.Font(None, 80)
    titleText = titleFont.render("Amazeing Puzzles", 1, white, dispSurf)
    dispSurf.blit(titleText, (150, 175))
    nameFont = pygame.font.Font(None, 50)
    nameText = nameFont.render("By: Will Krietemeyer", 1, white, dispSurf)
    dispSurf.blit(nameText, (235, 250))
    genFont = pygame.font.Font(None, 30)
    genText = ("Navigate the maze and find clues along your way!", "Your goal is to complete the 3 levels as quick as possible.", "In levels 2+3, you can go through the wall in some spots.", "Arrow keys to move, R to reset level, T to see the timer, Esc to quit.", "Press any key to continue.")
    y = 300
    for i in range(len(genText)):
        instSurf = genFont.render(genText[i], 1, white)
        instRect = instSurf.get_rect()
        y += 10 # 10 pixels will go in between each line of text.
        instRect.top = y
        instRect.centerx = midWidth
        y += instRect.height # Adjust for the height of the line.
        dispSurf.blit(instSurf, instRect)
    pygame.display.update()
    while True: # Main loop for the start screen.
        for event in pygame.event.get():
            if event.type == QUIT:
                end = True
            elif event.type == KEYDOWN:
                if event.key == K_ESCAPE:
                    end = True
                pygame.draw.rect(dispSurf, hide, (0, 0, winWidth, winHeight))
                pygame.display.update()
                return # user has pressed a key, so return.
 
def checkAnswers(): #Have loop, have them press the corrasponding number to the answer, and shuffle them
    global numOfAnswers, levelNum
    numOfAnswers = int(numOfAnswers)
    items1 = answers[0:numOfAnswers]
    items2 = answers[numOfAnswers:(numOfAnswers*2)]
    shuffled2 = answers[numOfAnswers:(numOfAnswers*2)]
    random.shuffle(shuffled2)
    items3 = answers[(numOfAnswers*2):(numOfAnswers*3)]
    shuffled3 = answers[(numOfAnswers*2):(numOfAnswers*3)]
    random.shuffle(shuffled3)
    if levelNum == 3:
        items4 = answers[(numOfAnswers*3):(numOfAnswers*4)]
        shuffled4 = answers[(numOfAnswers*3):(numOfAnswers*4)]
        random.shuffle(shuffled4)
    for x in range(0, numOfAnswers):
        print("Which of the following corrasponds to "+items1[x]+"?")
        for y in range(0, numOfAnswers):
            print(str(y+1)+". "+shuffled2[y])
        out = -1
        while out == -1: # Main loop for the start screen.
            for event in pygame.event.get():
                if event.type == QUIT:
                    end = True
                    return "leave"
                elif event.type == KEYDOWN:
                    if event.key == K_ESCAPE:
                        end = True
                        return "leave"
                    elif event.key == K_BACKSPACE:
                        return "gave up"
                    elif event.key == K_1:
                        out = 0
                    elif event.key == K_2:
                        out = 1
                    elif event.key == K_3:
                        out = 2
                    elif (event.key == K_4 and levelNum > 1):
                        out = 3
                    elif (event.key == K_5 and levelNum == 2):
                        out = 4
        if (not shuffled2[out] == items2[x]):
            print("Incorrect, please try again later.")
            return "incorrect"
        print("Which of the following corrasponds to "+items1[x]+"?")
        for y in range(0, numOfAnswers):
            print(str(y+1)+". "+shuffled3[y])
        out = -1
        while out == -1: # Main loop for the start screen.
            for event in pygame.event.get():
                if event.type == QUIT:
                    end = True
                    return "leave"
                elif event.type == KEYDOWN:
                    if event.key == K_ESCAPE:
                        end = True
                        return "leave"
                    elif event.key == K_BACKSPACE:
                        return "gave up"
                    elif event.key == K_1:
                        out = 0
                    elif event.key == K_2:
                        out = 1
                    elif event.key == K_3:
                        out = 2
                    elif (event.key == K_4 and levelNum > 1):
                        out = 3
                    elif (event.key == K_5 and levelNum == 2):
                        out = 4
        if (not shuffled3[out] == items3[x]):
            print("Incorrect, please try again later.")
            return "incorrect"
        if levelNum == 3:
            print("Which of the following corrasponds to "+items1[x]+"?")
            for y in range(0, numOfAnswers):
                print(str(y+1)+". "+shuffled4[y])
            out = -1
            while out == -1: # Main loop for the start screen.
                for event in pygame.event.get():
                    if event.type == QUIT:
                        end = True
                        return "leave"
                    elif event.type == KEYDOWN:
                        if event.key == K_ESCAPE:
                            end = True
                            return "leave"
                        elif event.key == K_BACKSPACE:
                            return "gave up"
                        elif event.key == K_1:
                            out = 0
                        elif event.key == K_2:
                            out = 1
                        elif event.key == K_3:
                            out = 2
                        elif (event.key == K_4 and levelNum > 1):
                            out = 3
                        elif (event.key == K_5 and levelNum == 2):
                            out = 4
            if (not shuffled4[out] == items4[x]):
                print("Incorrect, please try again later.")
                return "incorrect"
    print("Congrats, you won!")
    levelNum += 1
    return "won"
 
pygame.display.update()
homeScreen()
levelNum = 1
t1 = time.time()
while (not(end) and levelNum != 4):
    pygame.draw.rect(dispSurf, hide, (0, 0, winWidth, winHeight))
    pygame.display.update()
    setupLevel(levelNum, levels, player)
    setupLogicPuzzle()
    hideClues()
    currentLevel = levelNum
    while (currentLevel == levelNum and not(end)):
        for event in pygame.event.get(): # event handling loop
            pygame.display.update()
            FPSClock.tick()
            if event.type == QUIT:
                # Player clicked the "X" at the corner of the window.
                end = True
            elif event.type == KEYDOWN:
                #Handle key presses
                if event.key == K_e:
                    print('How dare you press the "e" key!')
                elif event.key == K_ESCAPE:
                    end = True
                elif event.key == K_UP:
                    move("up")
                elif event.key == K_DOWN:
                    move("down")
                elif event.key == K_LEFT:
                    move("left")
                elif event.key == K_RIGHT:
                    move("right")
                elif event.key == K_r:
                    pygame.draw.rect(dispSurf, hide, (0, 0, winWidth, winHeight))
                    pygame.display.update()
                    setupLevel(levelNum, levels, player)
                elif event.key == K_t:
                    t2 = time.time()
                    print("Time taken so far: "+str('{0:.3g}'.format(t2-t1))+" seconds")
                elif event.key == K_p:
                    levelNum += 1
t2 = time.time()
print("Time it took you: "+str('{0:.3g}'.format(t2-t1))+" seconds")
pygame.quit()
print("Have a nice day!")
time.sleep(5)
