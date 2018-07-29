import com.sun.opengl.util.Animator;
import com.sun.opengl.util.GLUT;

import java.awt.Frame;
import java.awt.event.KeyEvent;
import static java.awt.event.KeyEvent.VK_R;
import static java.awt.event.KeyEvent.VK_UP;
import java.awt.event.KeyListener;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;
import java.util.Timer;
import java.util.TimerTask;
import javax.media.opengl.GL;
import static javax.media.opengl.GL.GL_LINE_LOOP;
import javax.media.opengl.GLAutoDrawable;
import javax.media.opengl.GLCanvas;
import javax.media.opengl.GLEventListener;
import javax.media.opengl.glu.GLU;
import static javax.swing.JOptionPane.showMessageDialog;


public class Snake3  implements GLEventListener, KeyListener {
int maxLength=3;
int snakeLength=3;
int fps=10;
boolean gameOver=false;
private GLUT glut=new GLUT();
private GLU glu = new GLU();

private int ROWS,COLS=40;
//private int COLS=40;
private final int FPS=10;

//***Snake Array Body ***\\
double[] posX={0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0};
double[] posY={2,4,6,8,10,12,14,0,0,0,0,0,0,0,0,0,0,0,0,0};

//Snake movement 
final int UP=1;
final int DOWN=-1;
final int RIGHT=2;
final int LEFT=-2;


int sDirection=RIGHT;

    public static void main(String[] args) {
        Snake3 snake=new Snake3();
        Frame frame = new Frame("Snake Game");
        GLCanvas canvas = new GLCanvas();
        canvas.addGLEventListener(new Snake3());
        frame.add(canvas);
        frame.setSize(600, 600);
        final Animator animator = new Animator(canvas);
        
        frame.addWindowListener(new WindowAdapter() {
            @Override
            public void windowClosing(WindowEvent e) {
                
                // exiting
                new Thread(new Runnable() {
                    public void run() {
                        animator.stop();
                        System.exit(0);
                    }
                }).start();
            }
        });
        // Center frame
        frame.setLocationRelativeTo(null);
        frame.setVisible(true);

        animator.start();
    }

    public void init(GLAutoDrawable drawable) {
        // Use debug pipeline
        // drawable.setGL(new DebugGL(drawable.getGL()));
        GL gl = drawable.getGL();
        System.err.println("INIT GL IS: " + gl.getClass().getName());
        // Enable VSync
        gl.setSwapInterval(1);
        // Setup the drawing area and shading mode
        gl.glClearColor(0.0f, 0.0f, 0.0f, 0.0f);
        gl.glShadeModel(GL.GL_SMOOTH); // try setting this to GL_FLAT and see what happens.
        drawable.addKeyListener(this);
        
    }
    
    
    public void reshape(GLAutoDrawable drawable, int x, int y, int width, int height) {
        GL gl = drawable.getGL();
        GLU glu = new GLU();
        if (height <= 0) { // avoid a divide by zero error!
            height = 1;

        }
        
        final float h = (float) width / (float) height;
        gl.glViewport(0, 0, width, height);
        gl.glMatrixMode(GL.GL_PROJECTION);
        gl.glLoadIdentity();
        gl.glOrtho(0.0,40.0, 0.0,40.0, -1, 1); //size 40x40
        gl.glMatrixMode(GL.GL_MODELVIEW);
        gl.glLoadIdentity();
    }
    
    
    //****** Food coordinates *******
        float fblx=0;
        float fbly=0;
        boolean food=true;
        //draw food function
        public void drawFood() 
        {
            if(food)
            {
                fblx=(float) (36*(Math.random()))+1f; //bottom left x
                fbly=(float) (36*(Math.random()))+1f; //bottom left y

            }
        }
        
        boolean obs1=true;
        boolean obs2=true;
        boolean obs3=true;
        float [] obs11=new float[2];
        float [] obs22=new float[2];
        float [] obs33=new float[2];
        public void drawObs1()
        {
        float[] val=new float[2];
            if(obs1)
          { 
            obs11[0]=(float) (30*(Math.random()))+0f; //bottom left x
            obs11[1]=(float) (30*(Math.random()))+0f;
          }
            //return val;
        }
        
        public float[] drawObs2()
        {
        float[] val=new float[2];
            if(obs2)
          { 
            obs22[0]=(float) (25*(Math.random()))+0f; //bottom left x
            obs22[1]=(float) (25*(Math.random()))+0f;
          }
            return val;
        }
        public float[] drawObs3()
        {
        float[] val=new float[2];
            if(obs3)
          { 
            obs33[0]=(float) (36*(Math.random()))+1f; //bottom left x
            obs33[1]=(float) (36*(Math.random()))+1f;
          }
            return val;
        }
    //Score variable - incremental
        int score=0;

    float rate=0.4f;
    public void display(final GLAutoDrawable drawable) {
       //randomFood();
        GL gl = drawable.getGL();
        // Clear the drawing area
        gl.glClear(GL.GL_COLOR_BUFFER_BIT | GL.GL_DEPTH_BUFFER_BIT);
        // Reset the current matrix to the "identity"
        gl.glLoadIdentity();
        // Move the "drawing cursor" around
      //gl.glTranslatef(-1.5f, 0.0f, -6.0f);
       gl.glTranslatef(0f, 0.0f, -1f);
       gl.glColor3f(1.0f,0.0f, 0.0f);
       gl.glLineWidth(1.0f); //Width of grid lines
        //size of quads
        float size=1.5f;
        //min value of the boundries
        float min=2;
        //max value of the boundries
        float max=38;
        /** Drawing the grid **/
        for(float x=min;x<max;x+=size)
        {
            for (float y=max; y>min; y-=size)
            {
                
                gl.glBegin(GL.GL_LINE_LOOP);
                    gl.glColor3f(1.0f, 1.0f, 1.0f);
                    gl.glVertex2d(x, y-size);
                    gl.glVertex2d(x, y);
                    gl.glVertex2d(x+size, y);
                    gl.glVertex2d(x+size, y-size);
                gl.glEnd();
            }
        }
        //Draw Food
        drawFood();
        //Food quad
            food=false;
            if(fblx<=3||fblx>=37||fbly<=3||fbly>=37) //higher score for food near boundries
            { 
                gl.glColor3f(1.0f,0.0f,0.0f); //red
                
            }
            
            else
                gl.glColor3f(0.0f, 1.0f, 0.0f);
                gl.glBegin(GL.GL_QUADS);
                
                gl.glVertex2d(fblx,fbly);
                gl.glVertex2d(fblx,fbly+size);
                gl.glVertex2d(fblx+size,fbly+size);
                gl.glVertex2d(fblx+size,fbly);
            gl.glEnd();
        //Adding load indentity twice
//        gl.glLoadIdentity();
//       
//       gl.glLoadIdentity();
     // float [] val=new float[2];
        
     
     gl.glFlush();
     //Collision Detection 
     //Check if the snake hits the food
     
        if(((Math.abs(posX[0]-fblx))<rate&&(Math.abs(posY[0]-fbly))<rate) ^
                ((Math.abs((posX[0])-fblx))<rate&&(Math.abs((posY[0]+size)-fbly+size))<rate) ^
                ((Math.abs((posX[0]+size)-fblx+size))<rate&&(Math.abs(posY[0]-fbly))<rate) ^
                ((Math.abs((posX[0]+size)-fblx+size))<rate&&(Math.abs((posY[0]+size)-fbly+size))<rate)) //&& (Math.abs(ulx-fulx))>0.02&&(Math.abs(blx-fblx))>0.02
        {
            food=true;
            snakeLength++;
            if(fblx<=3||fblx>=37||fbly<=3||fbly>=37)
                score+=2; 
            else
                score+=1;
            
            if(snakeLength>=posX.length-1) //Snake reached Maximum Length
            {
                 showMessageDialog(null, "Hurray! You Win\n you scored: "+score);
                 gameOver=true;
            }
            
        }
        //increasing the snakeLength/body
        for(int i= snakeLength-1;i>0;i--){
                 posX[i] =posX[i-1];
                 posY[i]= posY[i-1];
                 
        }
        
        //Check if the snake reached the boundries or not
     if(posX[0]+size<40&&posY[0]+size<40&& posX[0]>-1&&posY[0]>-1)
     {
        
         
     }
    
     else //Game Over
     {  
          gameOver=true;
//          posX[0]=20;
//          posY[0]=20;
     }
    
     
     if(score>=2)
     {
         drawObs1();
         float [] val=new float[2];
         val[0]=obs11[0];
         val[1]=obs11[1];
         
         gl.glColor3f(1.0f, 1.0f, 0);
         gl.glBegin(GL.GL_TRIANGLES);
            gl.glVertex2d(val[0],val[1]);
            gl.glVertex2d(val[0]+size/2,val[1]+size);
            gl.glVertex2d(val[0]+size,val[1]);
        gl.glEnd();
       obs1=false;
       if((Math.abs(val[0]-posX[0])<=0.5)&&(Math.abs(val[1]-posY[0])<=0.5) //Obstacle is hit
               )
           /* |(Math.abs((val[0]+size/2)-posX[0])<=0.5)&&(Math.abs((val[1]+size)-posY[0])<=0.5)^
               (Math.abs((val[0]+size)-posX[0])<=0.5)&&(Math.abs(val[1]-posY[0])<=0.5)
               */
       {
           gameOver=true;
       }
     }
     if(score>=5)
     {
         
        drawObs2();
        float [] val=new float[2];
        val[0]=obs22[0];
        val[1]=obs22[1];
        
         gl.glColor3f(1.0f, 1.0f, 0);
         gl.glBegin(GL.GL_TRIANGLES);
            gl.glVertex2d(val[0],val[1]);
            gl.glVertex2d(val[0]+size/2,val[1]+size);
            gl.glVertex2d(val[0]+size,val[1]);
        gl.glEnd();
        obs2=false;
        if((Math.abs(val[0]-posX[0])<=0.5)&&(Math.abs(val[1]-posY[0])<=0.5)) //Obstacle is hit
       {
           gameOver=true;
       }
        rate=0.4f;
     }
      if(score>=10)
     {
         
        drawObs3();
        float [] val=new float[2];
        val[0]=obs33[0];
        val[1]=obs33[1];
        
         gl.glColor3f(1.0f, 1.0f, 0);
         gl.glBegin(GL.GL_TRIANGLES);
            gl.glVertex2d(val[0],val[1]);
            gl.glVertex2d(val[0]+size/2,val[1]+size);
            gl.glVertex2d(val[0]+size,val[1]);
        gl.glEnd();
        
        obs3=false;
        if((Math.abs(val[0]-posX[0])<=0.5)&&(Math.abs(val[1]-posY[0])<=0.5))
       {
           gameOver=true;
       }
        rate=0.5f;
        //gameOver=true;
     }
     
    
     //Check the directions of the snake when it is changed by the keys
     if(sDirection==UP)
     {
        posY[0]+=rate;

     }
     if(sDirection==DOWN)
     {
        posY[0]-=rate;

     }
     if(sDirection==RIGHT)
     {
        posX[0]+=rate;

     }
      if(sDirection==LEFT)
     {
         posX[0]-=rate;

     }
      //creating the snake quad and increasing its body lenght.
        for (int i = 0; i < snakeLength; i++) {
             if (i==0) {
                gl.glColor3f(0.0f, 1.0f, 0.0f);
            }else{
             gl.glColor3f(0.0f, 0.0f, 1.0f);
             }
             System.out.println("GLBegin:"+i+" = "+posX[i]);
            gl.glBegin(GL.GL_QUADS);
            gl.glVertex2d(posX[i],posY[i]);
            gl.glVertex2d(posX[i],posY[i]+size);
            gl.glVertex2d(posX[i]+size,posY[i]+size);
            gl.glVertex2d(posX[i]+size,posY[i]);
        gl.glEnd();
        }
        for(int i=1;i<snakeLength;i++)
        {
            if(Math.abs(posX[0]-posX[i])<=0.2&& Math.abs(posY[0]-posY[i])<=0.2)
               gameOver=true; 
        }
     if(gameOver)
     {
         showMessageDialog(null, "Game Over\n you scored: "+score);
         System.exit(1);
     }
          
            }
    

    public void displayChanged(GLAutoDrawable drawable, boolean modeChanged, boolean deviceChanged) {

    }

  
    
    public void keyPressed(KeyEvent key)
     {
        //public void keyPressed(KeyEvent e) {
        //throw new UnsupportedOperationException("Not supported yet."); //To change body of generated methods, choose Tools | Templates.
        int keyCode=key.getKeyCode();
        switch (key.getKeyCode())
        {
            case VK_UP:
                if(sDirection!=DOWN)
                    sDirection=UP;
                break;
             case KeyEvent.VK_DOWN:
                 if(sDirection!=UP)
                     sDirection=DOWN;
                 break;
             case KeyEvent.VK_LEFT:
                 if(sDirection!=RIGHT)
                     sDirection=LEFT;
                 break;
             case KeyEvent.VK_RIGHT:
                 if(sDirection!=LEFT)
                 {
                     sDirection=RIGHT;
                 }
                 break;
//                 
}
     }
    public void keyTyped(KeyEvent e) {
       // throw new UnsupportedOperationException("Not supported yet."); //To change body of generated methods, choose Tools | Templates.
    }

    public void keyReleased(KeyEvent e) {
        //throw new UnsupportedOperationException("Not supported yet."); //To change body of generated methods, choose Tools | Templates.
    }
         }



