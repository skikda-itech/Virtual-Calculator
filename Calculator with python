import cv2 #just a library to import
from cvzone.HandTrackingModule import HandDetector


# Button est une classe pour la creation des objets representant les touches dans le contexte d une calculatrice
class Button:
    #on commence a construire notre button
    def __init__(self, pos, width, height, value):
        self.pos = pos #self est utilisé pour accéder aux attributs et aux méthodes de l'objet.
        self.width = width
        self.height = height
        self.value = value

    def draw(self, img):
        cv2.rectangle(img, self.pos, (self.pos[0] + self.width, self.pos[1] + self.height),
                      (250, 251, 217), cv2.FILLED)  # pour la background background calculatrice
        cv2.rectangle(img, self.pos, (self.pos[0] + self.width, self.pos[1] + self.height),
                      (50, 50, 50), 3)  #pour les bordure de la  calculatrice

        cv2.putText(img, self.value, (self.pos[0] + 25, self.pos[1] + 40), #La position où afficher le texte resultat a gauche
                    cv2.FONT_HERSHEY_PLAIN, 2, (0, 0, 255), 2)  # pour les valeurs values calculator

    def checkClicking(self, x, y):
        if (self.pos[0] < x < self.pos[0] + self.width and
                self.pos[1] < y < self.pos[1] + self.height):
            cv2.rectangle(img, self.pos, (self.pos[0] + self.width, self.pos[1] + self.height),
                          (255, 255, 255), cv2.FILLED)  # for the background calculator
            cv2.rectangle(img, self.pos, (self.pos[0] + self.width, self.pos[1] + self.height),
                          (50, 50, 50), 3)  # for the border calculator

            cv2.putText(img, self.value, (self.pos[0] + 25, self.pos[1] + 50),
                        cv2.FONT_HERSHEY_PLAIN, 3, (0, 0, 0), 4)
            return True
        else:
            return False

#ETAPE DES CALCULS
cap = cv2.VideoCapture(0)
detector = HandDetector(detectionCon=0.8, maxHands=1) #creation d objet
# creation des buttons
buttonListValue = [["1", "2", "3", "+"],
                   ["4", "5", "6", "-"],
                   ["7", "8", "9", "*"],
                   ["0", "/", ".", "="]]
buttonList = [] # pour stocker les boutons de la calculatrice grace des boucles
for x in range(4):
    for y in range(4):
        xPos = x * 70 + 350 #des multiples de 70 pixels, de manière à positionner les boutons dans une grille
        # starting from 350 pixel in the width
        yPos = y * 70 + 100  # starting from 100 pixel in the height
        buttonList.append(Button((xPos, yPos), 70, 70, buttonListValue[x][y]))
    # to store the whole equation from the calculator
    equation = ""
# to avoid duplicated value inside calculator in event writing
delayCounter = 0
while True:
    success, img = cap.read();
    img = cv2.flip(img, 1)
    # detection hand
    hand, img = detector.findHands(img, flipType=False)

    # to draw the border and background result placement(display) calculator
    cv2.rectangle(img, (350, 30), (350 + 280, 100),
                  (250, 251, 217), cv2.FILLED)
    cv2.rectangle(img, (350, 30), (350 + 280, 100),
                  (50, 50, 50), 3)

    for button in buttonList:
        button.draw(img)

    if (len(hand) > 0):
        lmList = hand[0]["lmList"] #landmark represente par coordonnes x et y
        distance, _, img = detector.findDistance(lmList[8], lmList[12], img)
        x, y = lmList[8]
        if (distance < 50):
            for button in buttonList:
                if (button.checkClicking(x, y) and delayCounter == 0):
                    if (button.value == "="):
                        equation = str(eval(equation))
                    else:
                        equation += button.value
                    delayCounter = 1
                    # avoid duplicates
    if (delayCounter != 0):
        delayCounter += 1
        # i did not add value into display calculator
        # after passing 10 frames
        if (delayCounter > 10):
            delayCounter = 0

    cv2.putText(img, equation, (355, 80),
                cv2.FONT_HERSHEY_PLAIN, 2, (50, 50, 50), 2)
    cv2.imshow("image", img)
    key = cv2.waitKey(1)
    if (key == ord("c")):  # to clear the display calculator
        equation = ""
    if key == ord('q'):  # to stop the program
        break
