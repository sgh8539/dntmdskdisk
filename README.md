다운로드 링크
github.com/algocon2020/Algo2020
https://drive.google.com/drive/folders/1aB7uDYu3Maf3zcMCYBuEb7HRT0RCxNXt


■ 차량의 수집 /제어 정보
control_driving 메소드의 sensing_info 파라미터를 통해 받는 정보는 다음과 같습니다.

sensing_info.to_middle
도로의 중앙차선으로부터의 차량까지의 직선 거리(m) 입니다.
이 값이 양의 값(+) 이면 차가 도로의 오른쪽에, 음수값(-) 이면 도로의 왼쪽에 위치함을 의미합니다.
Ex) to_middle : -10.73 | Type : float

sensing_info.collided
충돌했는지 여부. 장애물과 충돌상태에서 계속해서 가속을 하면 계속해서 False이며, 정지(속도 = 0) 하거나 충돌상태에서 벗어나면 False 로 바뀝니다.
Ex) collided : True | Type : bool

sensing_info.speed
현재 차량의 속도 (km/h) 를 나타냅니다.
Ex) speed : 10.51 | Type : float

sensing_info.moving_forward
목표지점을 항하여 정주행(True) 하고 있는지 역주행(False) 하고 있는 나타냅니다.
Ex) moving_forward : True | Type : bool for python
                                                          | Type : float for cpp or java

sensing_info.moving_angle
도로의 방향에 얼마나 정렬(align) 되어있는지를 말해주는 각 입니다. 가령, 이 값이 0 인 경우 도로와 평행하게 주행하고 있음을 나타내고,
-30도 이면 정주행 방향에서 왼쪽으로 30도 방향으로 나아가고 있다는 의미이며, +30도 이면 정주행 방향에서 오른쪽으로 30도 방향으로 가고 있다는 의미입니다.
여기서 +/- 는 각도를 의미하는 것이지, 도로 중앙차선의 오른쪽/왼쪽 기준이 아님을 주의하시기 바랍니다.
Ex) moving_angle : -72.5 | Type : float

sensing_info.track_forward_angles
현재 위치 기준으로 차량 전방의 10개 구간에 대한 각도를 배열로 알려줍니다. 한개의 구간은 10m 이며, 총 10 개의 정보를 미리 알려주므로 전방의 100 m 까지 정보를 나타내 주는 것이라고 볼 수 있습니다.
각도가 + 일 경우 현재 차량위치에서 정주행 기준으로 오른쪽 방향 으로 기울어지는 각도이며, - 일 경우 왼쪽으로 기울어지는 각도이다.
예를 들어 이 값이 다음과 같이 들어왔다면, 오른쪽으로 휘어지는 도로가 이어짐을 알 수 있으며, 휘어지는 정도는 각도의 차이를 통하여 판단할 수 있습니다.
Ex) track_forward_angles : [4, 8, 12, 16, 20, 27, 43, 52, 55, 58] | Type : list [int]

sensing_info.lap_progress
Goal 지점 대비 얼마나 진행이 되었는지 percentage 로 보여줍니다. 100 이 되면 완주 한 것입니다.
Ex) lap_progress : 5.43 | Type : float

sensing_info.track_forward_obstacles
전방 100m 까지의 장애물 정보를 배열로 알려줍니다. 

장애물이 없는 경우 배열 사이즈가 0 이며(empty), 장애물이 있으면 가까이 있는 것부터 차례로 배열에 추가 됩니다.
배열에 실려있는 정보는 장애물과의 거리, 그리고 장애물의 to_middle 정보입니다.
(to_middle 값은 도로의 왼쪽에 있으면 - 값, 오른쪽에서 있으면 + 값으로 표시)

장애물의 사이즈는 모든 맵에서 고정 길이 2 m 이며, to_middle 값 기준으로 좌우 1 m 라고 보시면 됩니다.
Ex) track_forward_obstacles : [{'dist': 10.72, 'to_middle': 2.93}] | Type : list 
 
 sensing_info.opponent_cars_info
전방 100m, 후방 100m 안에 있는 상대편 차량의 정보를 알려줍니다.
배열의 순서는 내 차와 가까이 있는 순으로 정렬하여 들어옵니다.

상대편 차량에 대해 주어지는 정보는 아래와 같습니다.
상대 차량의 이름
도로 중앙선 기준으로 내 차와의 거리 차이(전방에 있을 때는 +값, 후방에 있을 때는 -값)
상대방 차량이 현재 도로 중앙에서 얼마나 떨어져서 주행하고 있는지(to_middle 값)
상대편 차량의 속도

내 차량과 상대편 차량의 거리는 각 차의 중앙점을 기준으로 표시됩니다.
가령 상대방과 내 차의 거리가 +10 m 이고, 내 차와의 to_middle(중앙차로에서의 거리) 값이 비슷하다면 
각 차량의 길이를 고려했을 때 바로 내 차의 앞을 주행하고 있는 것이겠죠.

상대 차량이 전방에 있는 경우, 거리는 양수 값으로 들어옵니다. 숫자 값은 m 단위 입니다. 

상대 차량이 후방에 있는 경우, 거리는 음수값으로 들어옵니다. 

곡선인 도로인 경우에도, 아래의 이미지와 같이 도로 중앙선을 기준으로 거리를 측정하였습니다. 
Ex) opponent_cars_info : [{'car_name': 'Car2', 'dist': -0.1, 'to_middle': 2.0, 'speed': -0.0}] | Type : list

sensing_info.distance_to_way_points
전방 10개의 way-point 와 차량의 직선 거리를 제공합니다.
 Ex) distance_to_way_points : [2.98, 12.47, 22.42, 32.39, 42.34, 52.32, 62.26, 72.13, 81.83, 91.22] | Type : list [float64]

About road width
도로의 폭은 맵 별로 조금씩 차이가 납니다. 도로 이탈여부를 판단하기 위하여 도로폭을 사용하실 때에는 다음 변수 값을 사용하시기 바랍니다. 이 값은 도로 절반 폭에 차량 절반 폭을 더한 값이며, 만약 도로가 10m 폭의 도로이면, 절반 폭인 5 m + 차량 절반 폭(1.25m) 가 더해진 6.25 의 값을 가지고 있습니다.(부모 클래스에 멤버변수로 값을 담고 있기 때문에 위치에 상관없이 사용하실 수 있습니다.)
               # road half width + car half width
               (Java, cpp) sensing_info.half_road_limit
               (Python) self. half_road_limit