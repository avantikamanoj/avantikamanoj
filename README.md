import QtQuick 2.15
import QtQuick.Controls 2.15
import QtQuick.Layouts 1.15
import QtQuick.Shapes 1.15

ApplicationWindow {
    visible: true
    width: 400
    height: 400
    title: "Analog Clock"

    Rectangle {
        width: parent.width
        height: parent.height

        Timer {
            id: timer
            interval: 1000
            running: true
            repeat: true
            onTriggered: {
                updateClock()
            }
        }

        function updateClock() {
            var now = new Date();
            var seconds = now.getSeconds();
            var minutes = now.getMinutes();
            var hours = now.getHours() % 12;

            secondHand.rotation = seconds * 6; 
            minuteHand.rotation = minutes * 6 + seconds * 0.1; 
            hourHand.rotation = hours * 30 + minutes * 0.5;
        }

        Item {
            id: clock
            anchors.centerIn: parent

            Rectangle {
                width: 200
                height: 200
                color: "white"
                border.color: "black"
                anchors.centerIn: parent

                Shape {
                    ShapePath {
                        id: dial
                        startX: clock.width 
                        startY: 0
                        PathLine {
                            x: clock.width
                            y: clock.height 
                        }
                        PathArc {
                            centerX: clock.width 
                            centerY: clock.height 
                            radiusX: clock.width 
                            radiusY: clock.height 
                            startAngle: 0
                            sweepAngle: 360
                        }
                    }

                    ShapePath {
                        id: ticks
                        strokeStyle.width: 2
                        strokeStyle.color: "black"

                        
                        for (var i = 0; i < 12; ++i) {
                            var angle = i * 30;
                            var x1 = clock.width / 2 + 80 * Math.cos(angle * Math.PI / 180);
                            var y1 = clock.height / 2 + 80 * Math.sin(angle * Math.PI / 180);
                            var x2 = clock.width / 2 + 90 * Math.cos(angle * Math.PI / 180);
                            var y2 = clock.height / 2 + 90 * Math.sin(angle * Math.PI / 180);

                            PathLine {
                                x: x1
                                y: y1
                            }

                            PathLine {
                                x: x2
                                y: y2
                            }
                        }
                    }

                    ShapePath {
                        id: secondHand
                        strokeStyle.width: 2
                        strokeStyle.color: "red"

                        PathLine {
                            x: clock.width 
                            y: clock.height 
                        }

                        PathLine {
                            x: clock.width 
                            y: 20
                        }
                    }

                    ShapePath {
                        id: minuteHand
                        strokeStyle.width: 4
                        strokeStyle.color: "black"

                        PathLine {
                            x: clock.width 
                            y: clock.height 
                        }

                        PathLine {
                            x: clock.width 
                            y: 40
                        }
                    }

                    ShapePath {
                        id: hourHand
                        strokeStyle.width: 6
                        strokeStyle.color: "black"

                        PathLine {
                            x: clock.width 
                            y: clock.height 
                        }

                        PathLine {
                            x: clock.width 
                            y: 30
                        }
                    }
                }
            }
        }
    }
}
