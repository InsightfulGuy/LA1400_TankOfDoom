# LA1400_TankOfDoom
The robocode robot tank, final version.
```java

package EvansTristan;
import robocode.*;
import java.util.Random; //A package of the Random class to generate random numbers

public class TankOfDoom extends JuniorRobot {
  private Random rand = new Random();
  private boolean lockedOnTarget = false;		// A true or false boolean to indicate if the robot has locked on to a target
  
	public void run() {
		setColors(green, brown, black); //Colors of the Tank
		while(true) {
      searchForRobots();	// Call the searchForRobots method to search for any robots in the area
		  turnGunRight(360);
		// Generate a random number between 0 and 1 and use it to determine the next action of the robot
		if (rand.nextDouble() < 0.5) {   // If the random number is less than 0.5, move the robot ahead 100 units
        ahead(100);
      } else {
        back(200);
      }
      turnRight(180);
		}
	}

	public void onScannedRobot() {
	  double angle = scannedAngle + scannedVelocity; // Calculate the angle between the robot and the detected robot
	  turnGunTo((int)(angle));
	  if (scannedDistance < 100) {   // If the distance between the robot and the detected robot is less than 100 units
		  lockedOnTarget = true; // Set the lockedOnTarget flag to true
		  fire(1);
	  } else if (scannedDistance > 100) {
		  turnRight(scannedAngle);
		  ahead(scannedDistance - 200);
	  }
	}

	public void onHitByBullet() {
		turnLeft(45);
		back(50);
	}
	
	public void onHitWall() {
		turnRight(90);
		ahead(50);
	}
	
	public void searchForRobots() {
    int step = 100;
    int angle = 0;
    
    while (angle < 360) {   // Keep scanning in a 360 degree circle
        turnGunRight(angle);  // Turn the gun to the next scanning angle
        if (scannedDistance == 5000) {	// If the robot has not found any other robots in its current scan
            turnRight(90);
            ahead(step);
            step *= 2;
        } else {
            return;
        }
        angle += 45; // Increase the scanning angle by 45 degrees for the next scan
    }
  }
}
```
