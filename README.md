# Hand-Gesture
# importing packages
import cv2
import mediapipe as mp

# used to convert protobuf message to a dictionary
from google.protobuf.json_format import MessageToDict

# building the model
mpHands = mp.solutions.hands
hands = mpHands.Hands(
static_image_mode=False,
min_detection_confidence=0.75,
min_tracking_confidence=0.75,
max_num_hands=2)

# reading from webcam
webcam = cv2.VideoCapture(0)

while True:
success, img = webcam.read()

# flipping the image for model
original_img=img.copy() # copying the original image
img = cv2.flip(img, 1)

# converting to RGB for model (model needs RGB img)
RGB_img = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)

# passing the image to the model
results = hands.process(RGB_img)

# if there is any result (if any hand is detected)
if results.multi_hand_landmarks:

if len(results.multi_handedness) == 2: # if two hands exist in the image
cv2.putText(original_img, &#39;Both Hands&#39;, (250, 56), cv2.FONT_HERSHEY_COMPLEX, 0.8, (0, 255, 0),
2)

else: # if only one hand exists in the image
for i in results.multi_handedness:
label = MessageToDict(i)[&#39;classification&#39;][0][&#39;label&#39;]

if label == &#39;Left&#39;:
cv2.putText(original_img, f&#39;{label} Hand&#39;, (20, 56), cv2.FONT_HERSHEY_COMPLEX, 0.8, (0,
255, 0), 2)

if label == &#39;Right&#39;:
cv2.putText(original_img, f&#39;{label} Hand&#39;, (460, 56), cv2.FONT_HERSHEY_COMPLEX, 0.8, (0,
255, 0), 2)

cv2.imshow(&#39;image&#39;, original_img)
if cv2.waitKey(1) &amp; 0xff == ord(&#39;q&#39;):

break

webcam.release()
cv2.destroyAllWindows()
