import java.io.File;
import java.io.IOException;
import java.util.concurrent.TimeUnit;
import javax.sound.sampled.AudioInputStream;
import javax.sound.sampled.AudioSystem;
import javax.sound.sampled.Clip;
import javax.sound.sampled.LineUnavailableException;
import javax.sound.sampled.UnsupportedAudioFileException;
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.*;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;

/**
 *
 * @author kylea
 * 
 * 
 * 
 * This version dated 9.26 -- helped me secure my own preorder, 
 * I was using this to spam the "add to cart" from the "saved for later"
 * I ended up clicking "go to checkout", entering the pin and clicking "complete" myself
 * This project setup for to do the checkout: entering cc or any other info, but because of the freeze on failure I did it myself
 * bleh
 * 
 * you have to login if starting at step 7, where it will: look for the add to cart from saved later, then look for error. if it finds error it refreshes and tries again
 * there's more but meh.
 */
public class doTarget {
    
        WebDriver driver;
        WebDriverWait wait;
        WebDriverWait shortWait;
        int sleepLength = 45;
        boolean isUnavail = false;
        boolean isDone = false;
                
       public static void main(String[] args){
       doTarget tS = new doTarget();
   }

       public doTarget()  {
           System.setProperty("webdriver.chrome.driver", "Chromedriver\\chromedriver.exe");
           driver = new ChromeDriver();
           driver.get("https://www.target.com"); //if it adds or i get it to add..
           wait = new WebDriverWait(driver, 10);
           shortWait = new WebDriverWait(driver,4);
           try{
             TimeUnit.SECONDS.sleep(15);  
           }
           catch(Exception e){
               
           }
           //this.doIt(0);
           this.doIt(7); // if it adds or i get it to add..
       }
       public void doIt(int SkipStep){
            System.out.println("Calling doIt, isDone is " + isDone);
            
            
            
            int Step = 1;
            
            
              while  (!isDone){
                  try{
                      if (SkipStep == 0){
                        driver.get("https://www.target.com/p/playstation-5-console/-/A-81114595");
                        //driver.get("https://www.target.com/p/playstation-5-hd-camera/-/A-81114478");

                       
                      }
                      if (SkipStep == 0 || SkipStep == 2){
                        String preorderXpath ="//button[@data-test='preorderButton']";
                        Step = 2;
                        wait.until(ExpectedConditions.presenceOfElementLocated (By.xpath(preorderXpath)));
                        System.out.println("Found preorder Button ");
                    }
                      if (SkipStep == 0 || SkipStep <= 3){
                        Step = 3;
                        WebElement preorderButton = driver.findElement(By.xpath("//button[@data-test='preorderButton']"));
                        System.out.println("Trying to Click Preorder Button!!!!");
                        preorderButton.click();
                      }
                      if (SkipStep == 0 || SkipStep <= 4){
                      Step = 4;
                      String declineCoverageXPATH = "//button[@data-test=\'espModalContent-declineCoverageButton\']";
                      wait.until(ExpectedConditions.presenceOfElementLocated (By.xpath(declineCoverageXPATH)));
                      WebElement declineCoverageButton = driver.findElement(By.xpath(declineCoverageXPATH));
                      declineCoverageButton.click();
                      }
                      if (SkipStep == 0 || SkipStep <= 5){
                      Step = 5;
                      String viewCartXPATH = "//button[@data-test=\'addToCartModalViewCartCheckout\']";
                      wait.until(ExpectedConditions.presenceOfElementLocated (By.xpath(viewCartXPATH)));
                      WebElement viewCartButton = driver.findElement(By.xpath(viewCartXPATH));
                      viewCartButton.click();
                      }
                      if (SkipStep == 0 || SkipStep <= 6){
                       Step = 6;
                      String checkoutXPATH = "//button[@data-test=\'checkout-button\']";
                      wait.until(ExpectedConditions.presenceOfElementLocated (By.xpath(checkoutXPATH)));
                      WebElement checkoutButton = driver.findElement(By.xpath(checkoutXPATH));
                      checkoutButton.click();
                      }
                      
                      if (SkipStep == 7){
                      
                      
                      Step = 7;
                      String addX = "(//button[@data-test='moveSFLToCartBtn'])[1]";
                      wait.until(ExpectedConditions.presenceOfElementLocated (By.xpath(addX)));
                      WebElement doAdd = driver.findElement(By.xpath(addX));
                      doAdd.click();
                      }
                      /*
                      Step = 8;
                      String shipping ="(//input[contains(@id,'ulfillment-options')])[1]";
                      wait.until(ExpectedConditions.presenceOfElementLocated (By.xpath(shipping)));
                      WebElement doShipping = driver.findElement(By.xpath(shipping));
                      doShipping.click();
                      */
                      //otherwise 8 start here -.-
                      if (SkipStep <= 8){
                        Step = 8;
                        String error ="//*[@data-test='form-error-alert-box']";
                        shortWait.until(ExpectedConditions.presenceOfElementLocated (By.xpath(error)));
                        WebElement errorE = driver.findElement(By.xpath(error));
                        if (errorE.isDisplayed()){
                            System.out.println("Item(s) currently unavailable ");
                            isUnavail = true;
                            driver.navigate().refresh();

                            throw new NullPointerException("");
                        }
                      }
                          System.out.println("****ITEM AVAILABLE***");
                          //Only bad thing is now that the next step is delayed 5s looking for the above error
                   
                      //driver.navigate().refresh();
                      //If all goes well this should error at 8 then be recalled to start at 9
 
                      Step = 9;//I might have to write this so this is the first step. And if it fails here a step 9 then go back to 7.  B.C I might have to login manually.???
                      String checkoutB = "//button[@data-test='checkout-button']";
                        wait.until(ExpectedConditions.presenceOfElementLocated (By.xpath(checkoutB)));
                      WebElement doCh = driver.findElement(By.xpath(checkoutB));
                      doCh.click();
                      
                      
                      /*
                      //it might have a login screen around here which I think is blocked unless I try more testing on it lol
                      Step = 10;
                      String saveandContinue = "//button[@data-test='save-and-continue-button']";
                        wait.until(ExpectedConditions.presenceOfElementLocated (By.xpath(saveandContinue)));
                      WebElement savecB = driver.findElement(By.xpath(saveandContinue));
                      savecB.click();
                      
                      */
                      
                      
                      /* First time payment setup only
                        Step = 11;

                     String firstNameXpath = "//input[@id='full_name']";
                        wait.until(ExpectedConditions.presenceOfElementLocated (By.xpath(firstNameXpath)));
                        WebElement fNBt = driver.findElement(By.xpath(firstNameXpath));
                        fNBt.sendKeys("");

                        Step = 12;
                        String addressXpath = "//input[@id='address_line1']";
                        WebElement adBt = driver.findElement(By.xpath(addressXpath));
                        adBt.sendKeys("");

                        Step = 13;
                        //input[@id='']
                        String zipcodeXpath = "//*[@id='zip_code']";
                        WebElement ziBt = driver.findElement(By.xpath(zipcodeXpath));
                        ziBt.clear();
                        ziBt.sendKeys("");

                        Step = 14;
                        //input[@id='city']
                        String cityXpath = "//input[@id='city']";
                        WebElement ciBt = driver.findElement(By.xpath(cityXpath));
                        ciBt.clear();
                        ciBt.sendKeys("");


                        Step = 15;
                        //*[@id='state']
                        String stateXpath = "//*[@id='state']";
                        WebElement stBt = driver.findElement(By.xpath(stateXpath));
                        //stBt.click();
                        stBt.sendKeys(""); //JS Set value?

                        Step = 16;
                        //input[@id='mobile']
                        String phoneXpath = "//*[@id='mobile']";
                        WebElement phBt = driver.findElement(By.xpath(phoneXpath));
                        phBt.sendKeys("");

                        Step = 17;
                        //button[@data-test='saveButton']
                        String continueXpath = "//button[@data-test='saveButton']";
                         wait.until(ExpectedConditions.presenceOfElementLocated (By.xpath(continueXpath)));
                        WebElement continueB = driver.findElement(By.xpath(continueXpath));
                        continueB.click();
                         */
                      
                      
                      //If it fails here i should do a shortwait if not her ethen skip to 19
                     
                      if (SkipStep<= 18){
                        Step = 18;
                        //input[@id='creditCardInput-cardNumber']
                        String cardXpath = "//input[@id='creditCardInput-cardNumber']";
                        shortWait.until(ExpectedConditions.presenceOfElementLocated (By.xpath(cardXpath)));
                        WebElement cardB = driver.findElement(By.xpath(cardXpath));
                        //cardB.sendKeys("");  //editme

                      Step = 19;
                      
                      String verfiyX = "//button[@data-test='verify-card-button']";
                       shortWait.until(ExpectedConditions.presenceOfElementLocated (By.xpath(verfiyX)));
                      WebElement verifyB = driver.findElement(By.xpath(verfiyX));
                      //verifyB.click();
                      }
                      
                      Step = 20;
                      
                       String CCVXp = "//input[@id='creditCardInput-cvv']";
                        wait.until(ExpectedConditions.presenceOfElementLocated (By.xpath(CCVXp)));
                       WebElement CcvIn = driver.findElement(By.xpath(CCVXp));
                       //CcvIn.sendKeys(""); //editme
                      
                        /* only for CC
                      Step = 21;
                     
                      String verfiyXX = "//button[@data-test='save-and-continue-button']";
                       wait.until(ExpectedConditions.presenceOfElementLocated (By.xpath(verfiyXX)));
                      WebElement verifyBB = driver.findElement(By.xpath(verfiyX));
                      verifyBB.click();
                      */
                      
                      Step = 22;
                     
                      String verfiyXXX = "//button[@data-test='placeOrderButton']";
                       wait.until(ExpectedConditions.presenceOfElementLocated (By.xpath(verfiyXXX)));
                      WebElement verifyBBB = driver.findElement(By.xpath(verfiyXXX));
                      //verifyBBB.click();
                      
                      
                      
                      
                      System.out.println("WTF0000000000");
                      String filePath = "sound\\alarm.wav"; 
                       AudioInputStream alarm =  AudioSystem.getAudioInputStream(new File(filePath).getAbsoluteFile()); 
                        Clip clip = AudioSystem.getClip(); 
                        clip.open(alarm); 
                        clip.loop(Clip.LOOP_CONTINUOUSLY); 
                        
                        isDone = true;
                      
                  }
                  catch(Exception e){
                      System.out.println("Failed at step: " + Step);
          
                        
                      try{
                        String failedPath;
                        boolean loop = false;
                        boolean doAudio = true;
                        if (Step < 4){
                            failedPath = "sound\\speech.wav";
                        }
                        else if (Step == 8){
                            //loop = true;
                             sleepLength = 10; //sigh i dont want to ddos. There's no point in short wait. It can be 30s. The stock is serverside. I got in twice earlier.
                            failedPath = "sound\\alarm.wav"; 
                            doAudio = false;
                        }
                        else if (Step >= 9){//9 is cant find error and fails at finding checkout button. may throw false positive. it's ok.
                            sleepLength = 0;
                            failedPath = "sound\\alarm.wav";
                            isDone = true;
                            doAudio = true;
                            //driver.navigate().refresh();
                        }
                        else{
                            failedPath = "sound\\alarm.wav"; 
                            if (Step <= 3){
                                loop = true;
                            }
                        }
                       if (doAudio && Step != 8){
                           System.out.println("WTFF!!!!!!");
                        AudioInputStream failed =  AudioSystem.getAudioInputStream(new File(failedPath).getAbsoluteFile()); 
                         Clip failedClip = AudioSystem.getClip(); 
                         failedClip.open(failed); 
                         failedClip.start();
                         if (loop){
                             failedClip.loop(Clip.LOOP_CONTINUOUSLY); 
                         }
                       }
                       TimeUnit.SECONDS.sleep(sleepLength);
                      }
                      catch(IOException | InterruptedException | LineUnavailableException | UnsupportedAudioFileException ee){
                          System.out.println("exception " + ee);
                      }
                       if (Step <= 2){
                        System.out.println("Didn't find preorder button. Reloading page.");
                        this.doIt(0);
                       }
                     // driver.quit();
                      //System.exit(0);
                  }
                  if ( ( Step >= 4 && Step < 7 )|| Step == 8 || Step == 20) {
                      int tempStep = Step;
                      if (tempStep == 8){
                          if (isUnavail == true){
                               tempStep = tempStep - 1;
                               isUnavail = false;//reset var
                          }
                          else{
                              tempStep = tempStep + 1;
                              if (Step == 20){
                                  tempStep = tempStep + 1;
                              }
                              System.out.println("****ITEM AVAILABLE***");
                          }   
                      }
                      System.out.println("Failed at step %s" + Step + " retrying the loop starting at" +
