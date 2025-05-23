# CSCI-3290-Assignment-2-Color-Transfer-Contrast-Preserving-Decolorization-solved

Download Here: [CSCI 3290 Assignment 2 – Color Transfer &amp; Contrast Preserving Decolorization solved](https://jarviscodinghub.com/assignment/assignment-2-color-transfer-contrast-preserving-decolorization-solution/)

For Custom/Original Work email jarviscodinghub@gmail.com/whatsapp +1(541)423-7793

I. Backgrounds
Image A Image B Color Transfer
Color Transfer – Transfer color style from one image to another. For example, we want to change the
appearance of Image A (ocean_day) into the style of Image B (ocean_sunset). Above is one effect generated
by “Color Transfer between images” [6].
Decolorization – the process to transform a color image to a grayscale one – is a basic tool in digital printing,
stylized black-and-white photography, and in many single channel image processing applications.
2
In this assignment, you will implement approaches aiming at transferring color and maximally preserving the
original color contrast in decolorization. To start your assignment, you will use test images and the skeleton
code provided by us. You are also encouraged to use your own pictures after coding is finished.
II. Details
Part 1: Color Transfer between Images
We call the image that we want to change “source image” img_src, and the desired image “target image”
img_tar. Our algorithm follows that of “Color Transfer between images” [6].
The goal is to transfer source image’s color to the target image. The modifications need to be done in the
colorspace named “Lab” color space.
Hint: you CANNOT use build-in functions to do the colorspace transformation.
Step 1: Convert input img_src & img_tar into Lab- color space
1. From RGB to LMS: for each color (R,G,B) in RGB color space.
2. From LMS to log-LMS
Attention: (L,M,S) values may be very close to 0, then log-LMS will be –inf. Please use x =
log(max(eps,x)) instead of x = log(x) for numerical stability (eps is a very small float number)
3. From log-LMS to Lab color space
Step 2: Calculate Mean and Standard Deviation for each channel of source and target image in Lab- color
space
??
, ??
, ?? are 3 channels of img_src
??
, ??
, ?? are 3 channels of img_tar
CSCI 3290 Assignment 2
3
〈??
〉,〈??
〉,〈??
〉 are mean value of img_src’s 3 channels
〈??
〉,〈??
〉,〈??
〉 are mean value of img_tar’s 3 channels
σs
?
, σs
?
, σs
?
are Standard Deviation value of img_src’s 3 channels
σt
?
, σt
?
, σt
?
are Standard Deviation value of img_tar’s 3 channels
Step 3: Color transfer
Get the 3 channels of the result image by the following
???? = (?? − 〈??
〉)
??
?
??
? + 〈??
〉
???? = (?? − 〈??
〉)
??
?
??
? + 〈??
〉
???? = (?? − 〈??
〉)
??
?
??
? + 〈??
〉
Step 4: Convert back to RGB color space
Please derive the equations to convert from Lab back to RGB color space.
Part 2: Basic Decolorization Algorithm
Suppose ? is the input color image and ? is the resulting gray-scale image. In order to achieve “contrast
preserving”, we need to make the difference between neighboring gray-scale pixels (??, ??) similar to the
contrast between original color pixels (??,??).
Our objective is to optimize a contrast preserving energy function
min
?
∑?,?(?? − ?? − ??,?)
2
(1)
g? and ?? are the gray values for two pixels ? and ?, which are neighbors. The neighboring is defined
either in x or y direction, i.e., for image position ? = (i, j) in the i-th row and j-th column, it has two
neighbors where ? = (? + 1,?) or ? = (?,? + 1). ??,? is the color difference between pixels ?? and ??.
Based on the Euclidian distance in Lab color space, color contrast is defined as
|??,?| = √(?? − ??)
2
+ (?? − ??)
2
+ (?? − ??)
2
sign(??,?) = sign(?? − ??)
The sign of ??,? is determined by L channel. Solving Equation (1) needs to take its first order derivative
with respect to each pixel value ??, and let the derivative equal to 0. This results in the following linear
equations
CSCI 3290 Assignment 2
4
?? − ?? = ??,?, for all pixels ? = (i, j) and their neighbors ? (2)
Since each pixel p has 2 neighbors q (one on the right of p with q = (? + 1,?), one immediately under
p with ? = (?,? + 1)). Therefore, we have 2 equations for each pixel p accordingly.
Equation (2) can be solved via constructing linear equations as
?? = Δ;
Where ? is a column vector form of the gray-scale image ? as
? = [?1,1, ?2,1, … , ??,1, ?1,2, ?2,2, … , ??,2, … , ?1,?, ?2,?, … , ??,?]
?
Here image ? is with resolution ? × ?. ??,?
is the i-th row, j-th column pixel value.
Δ is the vector form for all pixel pair (?, ?). ??,? as Δ = [?1,1
?
, ?2,1
?
, … ??,?
?
, ?1,1
?
, ?2,1
?
, … ??,?
?
]
?
. ??,?
? = ??,?
where ? = (?,?) and ? = (? + 1,?). ??,?
? = ??,? where ? = (?,?) and ? = (?,? + 1).
? is a highly sparse matrix with size 2?? × ?? with each row representing one neighborhood equation in
(2) corresponding to Δ.
So what you need to do is as follows:
Step 1: Convert the input image to Lab space
Do the same steps as those in part 1 of this assignment.
Step 2: Compute ??,? for each two neighboring pixels
Step 3: Constructing matrix A and vectors G and ?
To convert a matrix G into vector format, you can use Matlab function g = G(:); to convert a vector
to matrix, use Matlab function reshape().
You can use the sparse matrix functions sparse() and spdiags() in MATLAB to increase the speed
for constructing A.
We will discuss it in our tutorial in more details.
Step 4: Solve the objective function to get G
You can use inv() or ‘\’ in MATLAB. Please perform running time comparison for these two methods
in your report, and search information online to discuss their difference.
We show an example below for you to ensure that you are implementing the algorithm in a right way.
CSCI 3290 Assignment 2
5
Part 3: Decolorization Evaluation: Color Contrast Preserving Ratio (CCPR)
To quantitatively evaluate the decolorization algorithms, we propose a new metric. It is based on the finding
that if the color difference δ is smaller than a threshold τ, it becomes nearly invisible in human vision. The
task of contrast-preserving decolorization is therefore to maintain color change that is perceivable by human.
We define a color contrast preserving ratio (CCPR) as
???? =
#{(?, ?)|(?, ?) ∈ Ω, |?? − ??| ≥ ?}
||Ω||
where Ω is the set containing all neighboring pixel pairs with their original color difference ??,? ≥ ?. ||Ω||
is the number of pixel pairs in Ω.
#{(?, ?)|(?, ?) ∈ Ω, |?? − ??| ≥ ?} is the number of pixel pairs in Ω that are still distinctive after
decolorization.
For each image, please report the average CCPR score under different τ (ranging from 1 to 15).
Packages we provide:
1. Specification of Assignment 2
2. MATLAB skeleton code
3. Sample images for testing
4. Links for more data and related resource
III. Requirements
Your submission should contain the following:
CSCI 3290 Assignment 2
6
a) Codes including:
 \color_transfer\run_color_transfer.m – To start your algorithm
 \color_transfer\color_rgb2lab.m – Convert RGB to Lab
 \color_transfer\color_lab2rgb.m – Convert Lab to RGB
 \color_transfer\color_transfer.m – Transfer color in Lαβ

 \color2gray\run_cprgb2gray.m – To start your algorithm
 \color2gray\cprgb2gray.m – Part 2: Basic decolorization algorithm
 \color2gray\CCPR.m – Part 3: Method evaluation by CCPR
 Other codes you would like to add.
Remarks:
 You can use functions provided by MATLAB, its Toolboxes and libraries supplied by TAs (Unless
identified elsewhere). Note you need to describe the algorithms in your OWN words in the report.
 If you use others’ functions or codes, please clearly claim all of them in the report, and SUBMIT
them together with your assignment. Otherwise, it might be considered as plagiarism.
 You own work MUST be the majority of all your uploaded codes.
b) Reports:
 For this assignment, and all other assignments, you must make a report in HTML.
 In the report, you will describe your algorithm and any decisions you made to write your algorithm
a particular way. Then you will show and discuss the results of your algorithm for all the test cases.
 Discussion on algorithms’ efficiency.
 Discuss any extra credit you did if you want.
 Ideas and algorithms from others MUST be clearly claimed. List papers or links as reference if
needed.
 Feel free to add any other information you feel is relevant.
c) Hand-in
The folder you hand in must contain the following:
CSCI 3290 Assignment 2
7
1. README.docx – anything about the project that you want to tell the TAs, including brief
introduction of the usage of the codes
2. code/ – directory containing all your code for this assignment
– code/color_transfer/ – For Part 1
– code/color2gray/ – For Part 2 & 3
3. html/ – directory containing all your html report for this assignment (including images). Your web
page should only display compressed images (e.g. jpg or png).
4. html/index.html – home page for your results
Compressed the folder into -Asgn2.zip or -Asgn2.rar, and upload
it to e-Learning system.
IV. Scoring & Extra Bonus
a) Score
 +10 pts: Color transfer algorithm
 +25 pts: Basic decolorization algorithm
 +25 pts: The CCPR metric to evaluate the decolorization
 +20 pts: Report
 +20 pts: Extra credits
 -5*n pts: Losing 5 points every time (after the first) you do not follow the instructions to prepare the
uploading file format
b) Extra Credit
Three options for extra credit:
1. Implement a more advanced decolorization algorithm considering weak color order in
paper “Contrast Preserving Decolorization”. You can refer to the PPT to understand the
algoirthm. (up to 20 pts)
The parametric decolorization function is defined as ? = f(?), which is a mapping from
color image I to gray image g. The mapping function is further expressed as
?(?, ?, ?, ?) = ∑????
?
where ??
is an element in {?, ?, ?, ??, ??, ??, ?
2
, ?
2
, ?
2
}; ??
is the corresponding coefficients.
The weak color order is defined in Eq. (5) of the paper. Considering the weak order, the objective
function becomes
CSCI 3290 Assignment 2
8
?(?) = − ∑ ln{??,? exp {−
|∑ ??
? ? ?(?,?)−??,?|
2
2?2
} + (1 − ??,?)exp{−
|∑ ??
? ? ?(?,?)+??,?|
2
2?2
}} (?,?)∈ℕ (3)
To solve the Eq. (3) here (the same as Eq. (11) in the paper), the first order derivative can be
expressed as Eq. (13) in the paper. Define ?(??,?, ?
2
) = exp {−
|∑ ??
? ? ?(?,?)−??,?|
2
2?2
}.
The following algorithm solves ? iteratively.
Where ? and ? can be updated by the following equations.
Initial values for ? can be found in the paper.
2. Implement the even faster real-time decolorization method described in the PPT. (up to 15 pts)
3. Evaluate a total of three different decolorization algorithms (including what you have implemented
in this assignment) under CCPR metric. To find more decolorization algorithm, see here. Explain
each algorithm you have tried and analyze their performance. You can try whatever you think is
simple and reasonable (or some existing implementation that is theoretically different) for
decolorization. Include code you used or implemented in your package. (up to 10 pts)
V. Other Remarks
 Five late days total, to be spent wisely
 20% off from each extra late day

9
 The three assignments are to be completed INDIVIDUALLY on your own.
 You are encouraged to use methods in related academic papers.
 Absolutely NO sharing or copying code! NO sharing or copying reports! Offender will be given a
failure grade and the case will be reported to the faculty.
VI. Reference
[1] https://www.cs.northwestern.edu/~ago820/color2gray/color2gray.pdf
[2] https://www.cse.cuhk.edu.hk/~leojia/papers/decolorization_iccp12.pdf
[3] https://www.cse.cuhk.edu.hk/~leojia/papers/decolorization_ijcv14.pdf
[4] https://www.cse.cuhk.edu.hk/~leojia/projects/color2gray/c2gslides.pptx
[5] https://www.johndcook.com/blog/2009/08/24/algorithms-convert-color-grayscale/
[6] https://www.cs.utah.edu/~shirley/papers/ColorTransfer.pdf
