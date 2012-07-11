import java.awt.*;
import java.net.URL;

/**
 * A class that represents a picture.  This class inherits from SimplePicture
 *   and allows the student to add functionality and picture effects.  
 * 
 * @author Barb Ericson (ericson@cc.gatech.edu)
 * 	(Copyright Georgia Institute of Technology 2004)
 * @author Modified by Colleen Lewis (colleenl@berkeley.edu),
 * 	Jonathan Kotker (jo_ko_berkeley@berkeley.edu),
 * 	Kaushik Iyer (kiyer@berkeley.edu), George Wang (georgewang@berkeley.edu),
 * 	and David Zeng (davidzeng@berkeley.edu), for use in CS61BL, the data
 * 	structures course at University of California, Berkeley. 
 */
public class Picture extends SimplePicture 
{

	/////////////////////////// Static Variables //////////////////////////////

	// Different axes available to flip a picture.
	public static final int HORIZONTAL = 1;
	public static final int VERTICAL = 2;
	public static final int FORWARD_DIAGONAL = 3;
	public static final int BACKWARD_DIAGONAL = 4;

	// Different Picture objects for the bitmaps used in ASCII art conversion.
	private static Picture BMP_AMPERSAND;
	private static Picture BMP_APOSTROPHE;
	private static Picture BMP_AT;
	private static Picture BMP_BAR;
	private static Picture BMP_COLON; 
	private static Picture BMP_DOLLAR; 
	private static Picture BMP_DOT; 
	private static Picture BMP_EXCLAMATION; 
	private static Picture BMP_GRAVE; 
	private static Picture BMP_HASH;
	private static Picture BMP_PERCENT; 
	private static Picture BMP_SEMICOLON; 
	private static Picture BMP_SPACE; 	

	//////////////////////////// Constructors /////////////////////////////////

	/**
	 * A constructor that takes no arguments.
	 */
	public Picture () {
		super();  
	}

	/**
	 * Creates a Picture from the file name provided.
	 * 
	 * @param fileName The name of the file to create the picture from.
	 */
	public Picture(String fileName) {
		// Let the parent class handle this fileName.
		super(fileName);
	}

	/**
	 * Creates a Picture from the width and height provided.
	 * 
	 * @param width the width of the desired picture.
	 * @param height the height of the desired picture.
	 */
	public Picture(int width, int height) {
		// Let the parent class handle this width and height.
		super(width, height);
	}

	/**
	 * Creates a copy of the Picture provided.
	 * 
	 * @param pictureToCopy Picture to be copied.
	 */
	public Picture(Picture pictureToCopy) {
		// Let the parent class do the copying.
		super(pictureToCopy);
	}

	/**
	 * Creates a copy of the SimplePicture provided.
	 * 
	 * @param pictureToCopy SimplePicture to be copied.
	 */
	public Picture(SimplePicture pictureToCopy) {
		// Let the parent class do the copying.
		super(pictureToCopy);
	}

	/////////////////////////////// Methods ///////////////////////////////////

	/**
	 * @return A string with information about the picture, such as 
	 * 	filename, height, and width.
	 */
	public String toString() {
		String output = "Picture, filename = " + this.getFileName() + "," + 
		" height = " + this.getHeight() + ", width = " + this.getWidth();
		return output;
	}

	/////////////////////// PROJECT 1 BEGINS HERE /////////////////////////////

	/* Each of the methods below is constructive: in other words, each of 
	 * 	the methods below generates a new Picture, without permanently
	 * 	modifying the original Picture. */

	//////////////////////////////// Level 1 //////////////////////////////////

	/**
	 * Converts the Picture into grayscale. Since any variation of gray
	 * 	is obtained by setting the red, green, and blue components to the same
	 * 	value, a Picture can be converted into its grayscale component
	 * 	by setting the red, green, and blue components of each pixel in the
	 * 	new picture to the same value: the average of the red, green, and blue
	 * 	components of the same pixel in the original.
	 * 
	 * @return A new Picture that is the grayscale version of this Picture.
	 */
	public Picture grayscale() {
		Picture newPicture = new Picture(this);

		int pictureHeight = this.getHeight();
		int pictureWidth = this.getWidth();

		for(int x = 0; x < pictureWidth; x++) {
			for(int y = 0; y < pictureHeight; y++) {
				newPicture.setPixelToGray(x, y);
			}
		}
		return newPicture;
	}

	/**
	 * Helper method for grayscale() to set a pixel at (x, y) to be gray.
	 * 
	 * @param x The x-coordinate of the pixel to be set to gray.
	 * @param y The y-coordinate of the pixel to be set to gray.
	 */
	private void setPixelToGray(int x, int y) {
		Pixel currentPixel = this.getPixel(x, y);
		int average = currentPixel.getAverage();
		currentPixel.setRed(average);
		currentPixel.setGreen(average);
		currentPixel.setBlue(average);		
	}

	/**
	 * Test method for setPixelToGray. This method is called by
	 * the JUnit file through the public method Picture.helpersWork().
	 */
	private static boolean setPixelToGrayWorks()
	{
		Picture bg           = Picture.loadPicture("Creek.bmp");
		Pixel focalPixel     = bg.getPixel(10, 10);
		bg.setPixelToGray(10, 10);
		int goalColor        = (int) focalPixel.getAverage();
		int originalAlpha    = focalPixel.getColor().getAlpha();
		boolean redCorrect   = focalPixel.getRed() == goalColor;
		boolean greenCorrect = focalPixel.getGreen() == goalColor; 
		boolean blueCorrect  = focalPixel.getBlue() == goalColor;
		boolean alphaCorrect = focalPixel.getAlpha() == originalAlpha;
		return redCorrect && greenCorrect && blueCorrect && alphaCorrect;
	}

	/**
	 * This method provide JUnit access to the testing methods written 
	 * within Picture.java
	 */
	public static boolean helpersWork()
	{
		if (!Picture.setPixelToGrayWorks())
		{
			return false;
		}

		// You could put other tests here..

		return true;
	}
	
	/**
	 * General helper method for adjusting a pixel's color by an amount.
	 * 	
	 * @param x The x-coordinate of the pixel to be set to gray.
	 * @param y The y-coordinate of the pixel to be set to gray.
	 * @param color The color of the pixel to adjust.
	 * @param amount The amount to adjust that color by.
	 */
	private void adjustPixel(int x, int y, String color, int amount) {
		Pixel currentPixel = this.getPixel(x, y);
		if (color == "red") {
			currentPixel.setRed(currentPixel.getRed() + amount);
		} else if (color == "green") {
				currentPixel.setGreen(currentPixel.getGreen() + amount);
		} else if (color == "blue") {
			currentPixel.setBlue(currentPixel.getBlue() + amount);
		}
	}
	

	/**
	 * Converts the Picture into its photonegative version. The photonegative
	 * 	version of an image is obtained by setting each of the red, green,
	 * 	and blue components of every pixel to a value that is 255 minus their
	 * 	current values.
	 * 
	 * @return A new Picture that is the photonegative version of this Picture. 
	 */
	public Picture negate() {
		Picture newPicture = new Picture(this);

		int pictureHeight = this.getHeight();
		int pictureWidth = this.getWidth();

		for(int x = 0; x < pictureWidth; x++) {
			for(int y = 0; y < pictureHeight; y++) {
				newPicture.negateHelper(x, y);			
			}
		}
		return newPicture;
	}
	
	/**
	 * Helper method for negate. 
	 * 	
	 * @param x The x-coordinate of the pixel to be set to gray.
	 * @param y The y-coordinate of the pixel to be set to gray.
	 */
	
	private void negateHelper(int x, int y) {
		Pixel currentPixel = this.getPixel(x, y);
		currentPixel.setRed(255 - currentPixel.getRed());
		currentPixel.setGreen(255 - currentPixel.getGreen());
		currentPixel.setBlue(255 - currentPixel.getBlue());
		
	}	

	
	/**
	 * Creates an image that is lighter than the original image. The range of
	 * each color component should be between 0 and 255 in the new image. The
	 * alpha value should not be changed.
	 * 
	 * @return A new Picture that has every color value of the Picture increased
	 *         by the lightenAmount.
	 */
	public Picture lighten(int lightenAmount) {
		
		// throw exception if (lightenAmount > 255)
		Picture newPicture = new Picture(this);

		int pictureHeight = this.getHeight();
		int pictureWidth = this.getWidth();

		for(int x = 0; x < pictureWidth; x++) {
			for(int y = 0; y < pictureHeight; y++) {
				newPicture.adjustPixel(x, y, "red", lightenAmount);
				newPicture.adjustPixel(x, y, "green", lightenAmount);
				newPicture.adjustPixel(x, y, "blue", lightenAmount);
			}
		}
		return newPicture;
	}

	/**
	 * Creates an image that is darker than the original image.The range of
	 * each color component should be between 0 and 255 in the new image. The
	 * alpha value should not be changed.
	 * 
	 * @return A new Picture that has every color value of the Picture decreased
	 *         by the darkenenAmount.
	 */
	public Picture darken(int darkenAmount) {
		Picture newPicture = new Picture(this);

		int pictureHeight = this.getHeight();
		int pictureWidth = this.getWidth();

		for(int x = 0; x < pictureWidth; x++) {
			for(int y = 0; y < pictureHeight; y++) {
				newPicture.adjustPixel(x, y, "red", -darkenAmount);
				newPicture.adjustPixel(x, y, "green", -darkenAmount);
				newPicture.adjustPixel(x, y, "blue", -darkenAmount);
			}
		}
		return newPicture;
	}

	/**
	 * Creates an image where the blue value has been increased by amount.The range of
	 * each color component should be between 0 and 255 in the new image. The
	 * alpha value should not be changed.
	 * 
	 * @return A new Picture that has every blue value of the Picture increased
	 *         by amount.
	 */
	public Picture addBlue(int amount) {
		Picture newPicture = new Picture(this);

		int pictureHeight = this.getHeight();
		int pictureWidth = this.getWidth();

		for(int x = 0; x < pictureWidth; x++) {
			for(int y = 0; y < pictureHeight; y++) {
				newPicture.adjustPixel(x, y, "blue", amount);
			}
		}
		return newPicture;
	}
	
	/**
	 * Creates an image where the red value has been increased by amount. The range of
	 * each color component should be between 0 and 255 in the new image. The
	 * alpha value should not be changed.
	 * 
	 * @return A new Picture that has every red value of the Picture increased
	 *         by amount.
	 */
	public Picture addRed(int amount) {
		Picture newPicture = new Picture(this);

		int pictureHeight = this.getHeight();
		int pictureWidth = this.getWidth();

		for(int x = 0; x < pictureWidth; x++) {
			for(int y = 0; y < pictureHeight; y++) {
				newPicture.adjustPixel(x, y, "red", amount);
			}
		}
		return newPicture;
	}
	
	/**
	 * Creates an image where the green value has been increased by amount. The range of
	 * each color component should be between 0 and 255 in the new image. The
	 * alpha value should not be changed.
	 * 
	 * @return A new Picture that has every green value of the Picture increased
	 *         by amount.
	 */
	public Picture addGreen(int amount) {
		Picture newPicture = new Picture(this);

		int pictureHeight = this.getHeight();
		int pictureWidth = this.getWidth();

		for(int x = 0; x < pictureWidth; x++) {
			for(int y = 0; y < pictureHeight; y++) {
				newPicture.adjustPixel(x, y, "green", amount);
			}
		}
		return newPicture;
	}
	
	/** 
	 * @param x x-coordinate of the pixel currently selected.
	 * @param y y-coordinate of the pixel currently selected.
	 * @param background Picture to use as the background.
	 * @param threshold Threshold within which to replace pixels.
	 * 
	 * @return A new Picture where all the pixels in the original Picture,
	 * 	which differ from the currently selected pixel within the provided
	 * 	threshold (in terms of color distance), are replaced with the
	 * 	corresponding pixels in the background picture provided.
	 * 
	 * 	If the two Pictures are of different dimensions, the new Picture will
	 * 	have length equal to the smallest of the two Pictures being combined,
	 * 	and height equal to the smallest of the two Pictures being combined.
	 * 	In this case, the Pictures are combined as if they were aligned at
	 * 	the top left corner (0, 0).
	 */
	public Picture chromaKey(int xRef, int yRef, Picture background, int threshold) {
		//first set length/height of new picture to the smaller of the two pictures being combined.
		int newHeight;
		int newWidth;
		
		if (this.getHeight() < background.getHeight()) {
			newHeight = this.getHeight();
		} else {
			newHeight = background.getHeight();
		}
		// These if cases need to be separated as the width of one picture may be smaller than the other, while
		// the height of the same picture may be larger than the other.
		if (this.getWidth() < background.getWidth()) {
			newWidth = this.getWidth();
		} else {
			newWidth = background.getWidth();
		}
		// Construct a new Picture object with the smaller of the two dimensions. 
		Picture newPicture = new Picture(newWidth, newHeight);
		Pixel selectedPixel = this.getPixel(xRef,yRef);
		
		for (int x = 0; x < newPicture.getWidth(); x++) {
			for (int y = 0; y < newPicture.getHeight(); y++) {
				if (selectedPixel.colorDistance(this.getPixel(x, y).getColor())> threshold) {
					// take color from original picture at the pixel we just checked and copy it to newPicture.
					newPicture.getPixel(x, y).setColor(this.getPixel(x, y).getColor());
				} else {
					// take color from background picture at the pixel we just checked and copy it to newPicture.
					newPicture.getPixel(x, y).setColor(background.getPixel(x, y).getColor());
				}
			}
		}
		return newPicture;
	}

	//////////////////////////////// Level 2 //////////////////////////////////

	/**
	 * Rotates this Picture by the integer multiple of 90 degrees provided.
	 * 	If the number of rotations provided is positive, then the picture
	 * 	is rotated clockwise; else, the picture is rotated counterclockwise.
	 * 	Multiples of four rotations (including zero) correspond to no
	 * 	rotation at all.
	 * 
	 * @param rotations The number of 90-degree rotations to rotate this
	 * 	image by.
	 * 
	 * @return A new Picture that is the rotated version of this Picture.
	 */
	
	public Picture rotate(int rotations) {
		int rotateNum;
		Picture newPicture = new Picture(this);
		if (rotations >= 0){
			rotateNum = rotations % 4;
		}else{
			rotateNum = 4 + (rotations % 4);	
		}
		if (rotateNum == 0 || rotateNum == 4){
			return newPicture;
		}else{
			for (int x = 0; x < rotateNum; x++){
				newPicture = rotate90(newPicture);
			}
		}
		return newPicture;
	}
	
	/**
	 * Helper method for rotate() to rotate a picture by 90 degrees clockwise.
	 * 
	 * @param oldPicture The picture that will be rotated
	 *
	 */
	public Picture rotate90(Picture oldPicture) {
		int pictureHeight = oldPicture.getWidth();
		int pictureWidth = oldPicture.getHeight();

		Picture newPicture = new Picture(pictureWidth, pictureHeight);
		
		for(int x = 0; x < pictureWidth; x++){
			for(int y = 0; y < pictureHeight; y++){
				Pixel currentPixel = newPicture.getPixel(x, y);
				Pixel oldPixel = oldPicture.getPixel(y, oldPicture.getHeight() -1- x);
				currentPixel.setColor(oldPixel.getColor());
			}
		}
		return newPicture;
	}

	/**
	 * Flips this Picture about the given axis. The axis can be one of
	 * 	four static integer constants:
	 * 
	 * 	(a) Picture.HORIZONTAL: The picture should be flipped about
	 * 		a horizontal axis passing through the center of the picture.
	 * 	(b) Picture.VERTICAL: The picture should be flipped about
	 * 		a vertical axis passing through the center of the picture.
	 * 	(c) Picture.FORWARD_DIAGONAL: The picture should be flipped about
	 * 		an axis that passes through the north-east and south-west
	 * 		corners of the picture.
	 * 	(d) Picture.BACKWARD_DIAGONAL: The picture should be flipped about
	 * 		an axis that passes through the north-west and south-east
	 * 		corners of the picture.
	 * 
	 * @param axis Axis about which to flip the Picture provided.
	 * 
	 * @return A new Picture flipped about the axis provided.
	 */
	public Picture flip(int axis) {
		Picture newPicture = new Picture(this);
		
		switch (axis) {
		case Picture.HORIZONTAL:
			newPicture = newPicture.rotate(1);
			newPicture = flipVert(newPicture);
			newPicture = newPicture.rotate(-1);
			break;
		case Picture.VERTICAL: 
			newPicture = flipVert(newPicture);
			break;
		case Picture.FORWARD_DIAGONAL:
			newPicture = newPicture.rotate(-1);
			newPicture = flipVert(newPicture);
			break;
		case Picture.BACKWARD_DIAGONAL: 
			newPicture = newPicture.rotate(1);
			newPicture = flipVert(newPicture);
			break;
		default:
			break;
		}
		return newPicture;
	}

	/**
	 * Helper method for flip() to flip a Picture about a Vertical axis.
	 * 
	 * @param oldPicture The picture that will be rotated
	 *
	 */
	public Picture flipVert(Picture oldPicture) {
		int pictureHeight = oldPicture.getHeight();
		int pictureWidth = oldPicture.getWidth();

		Picture newPicture = new Picture(pictureWidth, pictureHeight);
		
		for(int x = 0; x < pictureWidth; x++){
			for(int y = 0; y < pictureHeight; y++){
				Pixel currentPixel = newPicture.getPixel(x, y);
				Pixel oldPixel = oldPicture.getPixel(pictureWidth-1-x, y);
				currentPixel.setColor(oldPixel.getColor());
			}
		}
		return newPicture;
	}
		
	/**
	 * @param threshold
	 *            Threshold to use to determine the presence of edges.
	 * 
	 * @return A new Picture that contains only the edges of this Picture. For
	 *         each pixel, we separately consider the color distance between
	 *         that pixel and the one pixel to its left, and also the color
	 *         distance between that pixel and the one pixel to the north, where
	 *         applicable. As an example, we would compare the pixel at (3, 4)
	 *         with the pixels at (3, 3) and the pixels at (2, 4). Also, since
	 *         the pixel at (0, 4) only has a pixel to its north, we would only
	 *         compare it to that pixel. If either of the color distances is
	 *         larger than the provided color threshold, it is set to black
	 *         (with an alpha of 255); otherwise, the pixel is set to white
	 *         (with an alpha of 255). The pixel at (0, 0) will always be set to
	 *         white.
	 */
	public Picture showEdges(int threshold) {
        int pictureHeight = this.getHeight();
        int pictureWidth = this.getWidth();

        Picture newPicture = new Picture(pictureWidth, pictureHeight);

        for(int x = pictureWidth-1; x >= 0 ; x--){
                for(int y = pictureHeight-1; y >= 0; y--){
                        boolean done = false;
                        Pixel currentPixel = newPicture.getPixel(x, y);
                        if(x != 0){
                                if ((int) this.getPixel(x,y).colorDistance(this.getPixel(x-1,y).getColor()) > threshold){
                                        currentPixel.setColor(Color.BLACK);
                                        done = true;
                                }else{
                                        currentPixel.setColor(Color.WHITE);
                                }
                        }
                        if(y != 0){
                                if ((int) this.getPixel(x,y).colorDistance(this.getPixel(x,y-1).getColor()) > threshold && !done){
                                        currentPixel.setColor(Color.BLACK);
                                        done = true;
                                }else if(!done){
                                        currentPixel.setColor(Color.WHITE);        
                                }
                        }
                        currentPixel.setAlpha(255);
                }
                newPicture.getPixel(0, 0).setColor(Color.white);
                newPicture.getPixel(0, 0).setAlpha(255);
        }
        return newPicture;

        //END

	}
	

	//////////////////////////////// Level 3 //////////////////////////////////

	/**
	 * @return A new Picture that is the ASCII art version of this Picture. To
	 *         implement this, the Picture is first converted into its grayscale
	 *         equivalent. Then, starting from the top left, the average color
	 *         of every chunk of 10 pixels wide by 20 pixels tall is computed.
	 *         Based on the average value obtained, this chunk will be replaced
	 *         by the corresponding ASCII character specified by the table
	 *         below.
	 *
	 *	       The ASCII characters to be used are available as Picture objects,
	 * 	       also of size 10 pixels by 20 pixels. The following characters
	 * 	       should be used, depending on the average value obtained:
	 * 	
	 * 	0 to 18: # (Picture.BMP_POUND)
	 * 	19 to 37: @ (Picture.BMP_AT)
	 * 	38 to 56: & (Picture.BMP_AMPERSAND)
	 * 	57 to 75: $ (Picture.BMP_DOLLAR)
	 * 	76 to 94: % (Picture.BMP_PERCENT)
	 * 	95 to 113: | (Picture.BMP_BAR)
	 * 	114 to 132: ! (Picture.BMP_EXCLAMATION)
	 * 	133 to 151: ; (Picture.BMP_SEMICOLON)
	 * 	152 to 170: : (Picture.BMP_COLON)
	 * 	171 to 189: ' (Picture.BMP_APOSTROPHE)
	 * 	190 to 208: ` (Picture.BMP_GRAVE)
	 * 	209 to 227: . (Picture.BMP_DOT)
	 * 	228 to 255: (Picture.BMP_SPACE)
	 * 
	 * We provide a getAsciiPic method to obtain the Picture object 
	 * 	corresponding to a character, given any of the static Strings
	 * 	mentioned above.
	 * 
	 * Note that the resultant Picture should be the exact same size
	 * 	as the original Picture; this might involve characters being
	 * 	partially copied to the final Picture. 
	 */
	public Picture convertToAscii() {
		int pictureHeight = this.getHeight();
		int pictureWidth = this.getWidth();
		Picture newPicture = new Picture(pictureWidth, pictureHeight);
		for(int x = 0; x < pictureWidth; x = x+10){
			for(int y = 0; y < pictureHeight; y = y + 20){
				int k = 0;
				int j = 0;
				int lastj = 0;
				int total = 0;
				int average = 0;
				while ((x+k)<pictureWidth && k < 10){
					if(j!=20){
						lastj = j;
						j = 0;
					}
					if(j == 20){
						j = 0;
					}
					while ((y+j)<pictureHeight && j < 20){
						Pixel currentPixel = this.getPixel(x+k, y+j);
						average = average + currentPixel.getAverage();
						total++;
						j++;
					}
					k++;
				}
				if(j == 20){
					lastj = j;
				}
				average = average/total;
				newPicture.asciiHelper(x, y, k, lastj, average);
			}
		}
		return newPicture;
	}

	/** Helper method for convertToAscii to set a segment to an ascii picture.
	 * 
	 * @param startx
	 * @param starty
	 * @param x
	 * @param y
	 * @param ascii
	 */
    private void asciiHelper(int startx, int starty,int x, int y, int ascii) {
            Picture asciiPicture = getAsciiPic(ascii);
            
            for(int k = 0; k < x; k++){
                    for(int j = 0; j < y; j++){
                            Pixel currentPixel = this.getPixel(startx + k,starty + j);
                            Pixel asciiPixel = asciiPicture.getPixel(k,j);
                            currentPixel.setColor(asciiPixel.getColor());
                    }
            }
            //END
    }

	/**
	 * Blurs this Picture. To achieve this, the algorithm takes a pixel, and
	 * sets it to the average value of all the pixels in a square of side (2 *
	 * blurThreshold) + 1, centered at that pixel. For example, if blurThreshold
	 * is 2, and the current pixel is at location (8, 10), then we will consider
	 * the pixels in a 5 by 5 square that has corners at pixels (6, 8), (10, 8),
	 * (6, 12), and (10, 12). If there are not enough pixels available -- if the
	 * pixel is at the edge, for example, or if the threshold is larger than the
	 * image -- then the missing pixels are ignored, and the average is taken
	 * only of the pixels available.
	 * 
	 * The red, blue, green and alpha values should each be averaged separately.
	 * 
	 * @param blurThreshold
	 *            Size of the blurring square around the pixel.
	 * 
	 * @return A new Picture that is the blurred version of this Picture, using
	 *         a blurring square of size (2 * threshold) + 1.
	 */
	public Picture blur(int blurThreshold) {
		Picture newPicture = new Picture(this);
		for (int x = 0; x < newPicture.getWidth(); x++) {
			for (int y = 0; y < newPicture.getHeight(); y++) {
				int redTotal = 0;
				int blueTotal = 0;
				int greenTotal = 0;
				int alphaTotal = 0;
				int numOfPixels = 0;
				int left_bound = x - blurThreshold;
				int right_bound = x + blurThreshold;
				int top_bound = y - blurThreshold;
				int bottom_bound = y + blurThreshold;
				if (left_bound < 0) {
					left_bound = 0;
				}
				if (right_bound >= this.getWidth()) {
					right_bound = this.getWidth() - 1;
				}
				if (top_bound < 0) {
					top_bound = 0;
				}
				if (bottom_bound >= this.getHeight()) {
					bottom_bound = this.getHeight() - 1;
				}
				for (int smallx = left_bound; smallx <= right_bound; smallx++){
					for (int smally = top_bound; smally <= bottom_bound; smally++) {
						redTotal = redTotal + this.getPixel(smallx, smally).getRed();
						blueTotal = blueTotal + this.getPixel(smallx, smally).getBlue();
						greenTotal = greenTotal + this.getPixel(smallx, smally).getGreen();
						alphaTotal = alphaTotal + this.getPixel(smallx, smally).getAlpha();
						numOfPixels++;
					}
				}
				newPicture.getPixel(x, y).setRed(Math.round(redTotal / numOfPixels));
				newPicture.getPixel(x, y).setBlue(Math.round(blueTotal / numOfPixels));
				newPicture.getPixel(x, y).setGreen(Math.round(greenTotal / numOfPixels));
				newPicture.getPixel(x, y).setAlpha(Math.round(alphaTotal / numOfPixels));
			}
		}
		return newPicture;
	}

	/**
	 * @param x x-coordinate of the pixel currently selected.
	 * @param y y-coordinate of the pixel currently selected.
	 * @param threshold Threshold within which to delete pixels.
	 * @param newColor New color to color pixels.
	 * 
	 * @return A new Picture where all the pixels connected to the currently
	 * 	selected pixel, and which differ from the selected pixel within the
	 * 	provided threshold (in terms of color distance), are colored with
	 * 	the new color provided. 
	 */
	public Picture paintBucket(int x, int y, int threshold, Color newColor) {
		Picture newPicture = new Picture(this);
		Picture newPicture2 = new Picture(this);
		newPicture2 = newPicture2.showEdges(threshold);
		newPicture.paintery(x , y, newPicture2, newColor, x, y, 0);
		return newPicture;
	}
	/**
	 * Helper method for paintBucket to paint the portion within the threshold.
	 * 
	 * 
	 *
	 */
	private void painterx(int x, int y, Picture threshold, Color newColor, int startx, int starty, int go) {
		this.getPixel(x,y).setColor(newColor);
		threshold.getPixel(x,y).setColor(Color.BLACK);
		
		if(x+1 < this.getWidth() && threshold.getPixel(x+1,y).getColor().equals(Color.WHITE) && x >= startx){ 
			this.painterx(x+1, y, threshold, newColor, startx, y, go);
			this.paintery(x+1, y, threshold, newColor, startx, y, go);
		}
		if(x-1 > 0 && threshold.getPixel(x-1,y).getColor().equals(Color.WHITE) && x <= startx ){
			this.painterx(x-1, y, threshold, newColor, startx, y, go);
			this.paintery(x-1, y, threshold, newColor, startx, y, go);
		}
	}
	private void paintery(int x, int y, Picture threshold, Color newColor, int startx, int starty, int adder) {
		this.getPixel(x,y).setColor(newColor);
		threshold.getPixel(x,y).setColor(Color.BLACK);
		
		if(y+1 < this.getHeight() && threshold.getPixel(x,y+1).getColor().equals(Color.WHITE) && y >= starty){
			this.paintery(x, y+1, threshold, newColor, x, starty, adder);
			this.painterx(x, y, threshold, newColor, x, starty, adder+1);
		}
		if(y-1 > 0 && threshold.getPixel(x,y-1).getColor().equals(Color.WHITE) && y <= starty  ){
			this.paintery(x, y-1, threshold, newColor, startx, starty, adder);
			this.painterx(x, y, threshold, newColor, startx, starty, adder+1);
		}
	}

	///////////////////////// PROJECT 1 ENDS HERE /////////////////////////////

	public boolean equals(Object obj) {
		if (!(obj instanceof Picture)) {
			return false;
		}

		Picture p = (Picture) obj;
		// Check that the two pictures have the same dimensions.
		if ((p.getWidth() != this.getWidth()) ||
				(p.getHeight() != this.getHeight())) {
			return false;
		}

		// Check each pixel.
		for (int x = 0; x < this.getWidth(); x++) {
			for(int y = 0; y < this.getHeight(); y++) {
				if (!this.getPixel(x, y).equals(p.getPixel(x, y))) {
					return false;
				}
			}
		}

		return true;
	}

	/**
	 * Helper method for loading a picture in the current directory.
	 */
	protected static Picture loadPicture(String pictureName) {
		URL url = Picture.class.getResource(pictureName);
		return new Picture(url.getFile().replaceAll("%20", " "));
	}

	/**
	 * Helper method for loading the pictures corresponding to each character
	 * 	for the ASCII art conversion.
	 */
	private static Picture getAsciiPic(int grayValue) {
		int asciiIndex = (int) grayValue / 19;

		if (BMP_AMPERSAND == null) {
			BMP_AMPERSAND = Picture.loadPicture("ampersand.bmp");
			BMP_APOSTROPHE = Picture.loadPicture("apostrophe.bmp");
			BMP_AT = Picture.loadPicture("at.bmp");
			BMP_BAR = Picture.loadPicture("bar.bmp");
			BMP_COLON = Picture.loadPicture("colon.bmp");
			BMP_DOLLAR = Picture.loadPicture("dollar.bmp");
			BMP_DOT = Picture.loadPicture("dot.bmp");
			BMP_EXCLAMATION = Picture.loadPicture("exclamation.bmp");
			BMP_GRAVE = Picture.loadPicture("grave.bmp");
			BMP_HASH = Picture.loadPicture("hash.bmp");
			BMP_PERCENT = Picture.loadPicture("percent.bmp");
			BMP_SEMICOLON = Picture.loadPicture("semicolon.bmp");
			BMP_SPACE = Picture.loadPicture("space.bmp");
		}

		switch(asciiIndex) {
		case 0:
			return Picture.BMP_HASH;
		case 1:
			return Picture.BMP_AT;
		case 2:
			return Picture.BMP_AMPERSAND;
		case 3:
			return Picture.BMP_DOLLAR;
		case 4: 
			return Picture.BMP_PERCENT;
		case 5:
			return Picture.BMP_BAR;
		case 6: 
			return Picture.BMP_EXCLAMATION;
		case 7:
			return Picture.BMP_SEMICOLON;
		case 8:
			return Picture.BMP_COLON;
		case 9: 
			return Picture.BMP_APOSTROPHE;
		case 10:
			return Picture.BMP_GRAVE;
		case 11:
			return Picture.BMP_DOT;
		default:
			return Picture.BMP_SPACE;
		}
	}

	public static void main(String[] args) {
		Picture initialPicture = new Picture(
				FileChooser.pickAFile(FileChooser.OPEN));
		initialPicture.explore();
	}

} // End of Picture class
