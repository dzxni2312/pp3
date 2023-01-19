/*
float light_ambient[] = { 0.0, 0.0, 0.0, 1.0 };
    float light_diffuse[] = { 1.0, 1.0, 1.0, 1.0 };
    float light_specular[] = { 1.0, 1.0, 1.0, 1.0 };
    GLfloat lightPosition[] = { -1.0, 0.0, 1.0, 0.0 };

    if (lightOn)
    {
        glEnable(GL_LIGHTING);
        glEnable(GL_LIGHT0);
    }

    glLightfv(GL_LIGHT0, GL_AMBIENT, light_ambient);
    glLightfv(GL_LIGHT0, GL_DIFFUSE, light_diffuse);
    glLightfv(GL_LIGHT0, GL_SPECULAR, light_specular);
    glLightfv(GL_LIGHT0, GL_POSITION, lightPosition);

void GLRenderer::DrawCone(double r, double h, int n)//kupa
{
    glBegin(GL_TRIANGLES);
    {
        float currAngle = 0;
        float angleStep = (2 * pi) / n;
        float L = sqrt(h * h + r * r);
        float ny = r / L;
        float nr = h / L;

        for (int i = 0; i < n; i++)
        {
            float nx = nr * cos(currAngle + angleStep / 2); // zasto + angleStep / 2??
            float nz = nr * sin(currAngle + angleStep / 2); // zato sto se vrh trougla nalazi na sredini stranie, tj. izmedju tacaka koje su definisane dole
            glNormal3f(nx, ny, nz);
            glVertex3f(0, h, 0);

            nx = nr * cos(currAngle); // normale racunamo sa nr; prva tacka bez rastojanja
            nz = nr * sin(currAngle);
            glNormal3f(nx, ny, nz);
            float x = r * cos(currAngle); //a koordinate sa obicno r 
            float z = r * sin(currAngle);
            glVertex3f(x, 0, z);

            nx = nr * cos(currAngle + angleStep); //druga tacka sa plus rastojanje, tj. ugao
            nz = nr * sin(currAngle + angleStep);
            glNormal3f(nx, ny, nz);
            x = r * cos(currAngle + angleStep);
            z = r * sin(currAngle + angleStep);
            glVertex3f(x, 0, z);

            currAngle += angleStep;
        }
    }
    glEnd();
}

void GLRenderer::DrawTube(double r1, double r2, double h, int n)//valjak
{
    glBegin(GL_QUAD_STRIP);
    {
        float texPart = 1.0 / (float)n; // kad imamo n segmenata nekog tela
        float angleStep = (2 * pi) / (float)n;  //kad nam treba neki ugao, okrugli objekat 
        float currAngle = 0;
        float y1 = h / 2, y2 = -h / 2;
        for (int i = 0; i <= n; i++)
        {
            float x1 = r1 * cos(currAngle);
            float z1 = r1 * sin(currAngle);
            float x2 = r2 * cos(currAngle);
            float z2 = r2 * sin(currAngle);

            float rDiff = r2 - r1;// ove dve pisem  da bih nasla ny i nr (normale)
            float L = sqrt(h * h + rDiff * rDiff);
            float ny = rDiff / L;
            float nr = h / L;
            float nx = nr * cos(currAngle);
            float nz = nr * sin(currAngle);

            glNormal3f(nx, ny, nz);

            glTexCoord2f(i * texPart, 0.0);
            glVertex3f(x1, y1, z1);

            glTexCoord2f(i * texPart, 1.0);
            glVertex3f(x2, y2, z2);

            currAngle += angleStep;
        }
    }
    glEnd();
}
void CGLRenderer::DrawCone(double r, double h,
    int nSeg, double texU, double texV, double texR)
{
    int i;
    double ang1, dAng1;
    double;
    dAng1 = 2 * PI / (double)nSeg;
    ang1 = 0;
    glBegin(GL_TRIANGLE_FAN);
    glColor3f(1.0, 1.0, 1.0);
    glTexCoord2f(texU, texV);
    glVertex3d(0.0, h, 0.0);
    for (i = 0; i < nSeg + 1; i++) {
        double x = r * cos(ang1);
        double y = 0;
        double z = r * sin(ang1);
        double tx = texR * x / r + texU;
        double ty = texR * z / r + texV;
        glTexCoord2f(tx, ty);
        glVertex3d(x, y, z);
        ang1 += dAng1;
    }
    glEnd();
}

void GLRenderer::DrawSpaceCube(double a)
{
    //glEnable(GL_DEPTH_TEST);
    glBindTexture(GL_TEXTURE_2D, spaceTex[4]);
    glBegin(GL_QUADS);
    {
        //top
        glTexCoord2f(0.0, 0.0);
        glVertex3f(-a / 2, a / 2, a / 2);
        glTexCoord2f(0.0, 1.0);
        glVertex3f(-a / 2, a / 2, -a / 2);
        glTexCoord2f(1.0, 1.0);
        glVertex3f(a / 2, a / 2, -a / 2);
        glTexCoord2f(1.0, 0.0);
        glVertex3f(a / 2, a / 2, a / 2);
    }
    glEnd();

    glBindTexture(GL_TEXTURE_2D, spaceTex[5]);
    glBegin(GL_QUADS);
    {
        //bottom
        glTexCoord2f(1.0, 1.0);
        glVertex3f(a / 2, -a / 2, a / 2);
        glTexCoord2f(1.0, 0.0);
        glVertex3f(a / 2, -a / 2, -a / 2);
        glTexCoord2f(0.0, 0.0);
        glVertex3f(-a / 2, -a / 2, -a / 2);
        glTexCoord2f(0.0, 1.0);
        glVertex3f(-a / 2, -a / 2, a / 2);
    }
    glEnd();

    glBindTexture(GL_TEXTURE_2D, spaceTex[0]);
    glBegin(GL_QUADS);
    {
        //front
        glTexCoord2f(0.0, 0.0);
        glVertex3f(-a / 2, a / 2, -a / 2);
        glTexCoord2f(0.0, 1.0);
        glVertex3f(-a / 2, -a / 2, -a / 2);
        glTexCoord2f(1.0, 1.0);
        glVertex3f(a / 2, -a / 2, -a / 2);
        glTexCoord2f(1.0, 0.0);
        glVertex3f(a / 2, a / 2, -a / 2);
    }
    glEnd();
    
    glBindTexture(GL_TEXTURE_2D, spaceTex[3]);
    glBegin(GL_QUADS);
    {
        //back
        glTexCoord2f(0.0, 0.0);
        glVertex3f(a / 2, a / 2, a / 2);
        glTexCoord2f(0.0, 1.0);
        glVertex3f(a / 2, -a / 2, a / 2);
        glTexCoord2f(1.0, 1.0);
        glVertex3f(-a / 2, -a / 2, a / 2);
        glTexCoord2f(1.0, 0.0);
        glVertex3f(-a / 2, a / 2, a / 2);
    }
    glEnd();

    glBindTexture(GL_TEXTURE_2D, spaceTex[1]);
    glBegin(GL_QUADS);
    {
        //left
        glTexCoord2f(0.0, 0.0);
        glVertex3f(-a / 2, a / 2, a / 2);
        glTexCoord2f(0.0, 1.0);
        glVertex3f(-a / 2, -a / 2, a / 2);
        glTexCoord2f(1.0, 1.0);
        glVertex3f(-a / 2, -a / 2, -a / 2);
        glTexCoord2f(1.0, 0.0);
        glVertex3f(-a / 2, a / 2, -a / 2);
    }
    glEnd();

    glBindTexture(GL_TEXTURE_2D, spaceTex[2]);
    glBegin(GL_QUADS);
    {
        //right
        glTexCoord2f(0.0, 0.0);
        glVertex3f(a / 2, a / 2, -a / 2);
        glTexCoord2f(0.0, 1.0);
        glVertex3f(a / 2, -a / 2, -a / 2);
        glTexCoord2f(1.0, 1.0);
        glVertex3f(a / 2, -a / 2, a / 2);
        glTexCoord2f(1.0, 0.0);
        glVertex3f(a / 2, a / 2, a / 2);
    }
    glEnd();
}

Definisati perspektivnu projekciju sa FOV = 50 i ispuniti funkcije PrepareScene(),
DrawScene() i Reshape() odgovarajućim OpenGL funkcijskim pozivima kako bi se
omogućilo dalje crtanje. Nacrtati tri linije dužine 10, koje kreću iz koordinatnog
početka i poklapaju sa koordinatnim osama. Neka je linija duž X‐ose crvena, linija
duž Y‐ose zelena, a duž Z‐ose plava. Preći na sledeću tačku tek kada koordinatne
ose budu vidljive. [10 poena]
void CGLRenderer::PrepareScene(CDC *pDC)
{
wglMakeCurrent(pDC‐>m_hDC, m_hrc);
glClearColor(1.0, 1.0, 1.0, 1.0);
glEnable(GL_DEPTH_TEST);
wglMakeCurrent(NULL, NULL);
}
void CGLRenderer::Reshape(CDC *pDC, int w, int h)
{
wglMakeCurrent(pDC‐>m_hDC, m_hrc);
glViewport(0, 0, (GLsizei)w, (GLsizei)h);
glMatrixMode(GL_PROJECTION);
glLoadIdentity();
gluPerspective(50, (double)w / (double)h, 0.1, 2000);
glMatrixMode(GL_MODELVIEW);
wglMakeCurrent(NULL, NULL);
}
void CGLRenderer::DrawScene(CDC *pDC)
{
wglMakeCurrent(pDC‐>m_hDC, m_hrc);
glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
glLoadIdentity();
// …
gluLookAt (10, 10, 10, 0, 0, 0, 0, 1, 0);
// Tripod
glLineWidth(2.0);
glBegin(GL_LINES);
glColor3f(1, 0, 0);
glVertex3f(0, 0, 0);
glVertex3f(10, 0, 0);
glColor3f(0, 1, 0);
glVertex3f(0, 0, 0);
glVertex3f(0, 10, 0);
glColor3f(0, 0, 1);
glVertex3f(0, 0, 0);
glVertex3f(0, 0, 10);
glEnd();
glFlush();
SwapBuffers(pDC‐>m_hDC);
wglMakeCurrent(NULL, NULL);
}
Napisati funkciju UINT CGLRenderer::LoadTexture(char* fileName), koja učitava teksturu
sa datim imenom (fileName) i vraća ID kreirane teksture. Korišćenjem ove funkcije u
okviru PrepareScene() učitati teksture: ShipT1.png, front.jpg, left.jpg, right.jpg, back.jpg,
top.jpg i bot.jpg. [15 poena]
UINT CGLRenderer::LoadTexture(char* fileName)
{
UINT texID;
DImage img;
img.Load(CString(fileName));
glPixelStorei(GL_UNPACK_ALIGNMENT, 4);
glGenTextures(1, &texID);
glBindTexture(GL_TEXTURE_2D, texID);
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_S, GL_REPEAT);
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_T, GL_REPEAT);
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR);
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR_MIPMAP_LINEAR);
glTexEnvf(GL_TEXTURE_ENV, GL_TEXTURE_ENV_MODE, GL_MODULATE);
gluBuild2DMipmaps(GL_TEXTURE_2D, GL_RGBA, img.Width(), img.Height(),
GL_BGRA_EXT, GL_UNSIGNED_BYTE, img.GetDIBBits());
return texID;
}
void CGLRenderer::PrepareScene(CDC *pDC)
{
wglMakeCurrent(pDC->m_hDC, m_hrc);
glClearColor(1.0, 1.0, 1.0, 1.0);
glEnable(GL_DEPTH_TEST);
m_texShip = LoadTexture("ShipT1.png");
m_texSpace[0]
= LoadTexture("front.jpg");
m_texSpace[1]
= LoadTexture("left.jpg");
m_texSpace[2]
= LoadTexture("right.jpg");
m_texSpace[3]
= LoadTexture("back.jpg");
m_texSpace[4]
= LoadTexture("top.jpg");
m_texSpace[5]
= LoadTexture("bot.jpg");
glEnable(GL_TEXTURE_2D);
wglMakeCurrent(NULL, NULL);
}
void CGLRenderer::DestroyScene(CDC *pDC)
{
wglMakeCurrent(pDC‐>m_hDC, m_hrc);
glDeleteTextures(1, &m_texShip);
glDeleteTextures(6, m_texSpace);
wglMakeCurrent(NULL,NULL);
if(m_hrc)
{
wglDeleteContext(m_hrc);
m_hrc = NULL;
}
}

Napisati funkciju void CGLRenderer::DrawSphere(double r, int nSeg, double texU,
double texV, double texR), koja iscrtava sferu poluprečnika r, sa nSeg segmenata po
latitudi i 2*nSeg segmenata po longitudi. Na sferu se primenjuje tekstura, tako što se
„projektuje“ odozgo (vidi sliku). texU i texV predstavljaju teksturne koordinate „vrha“
sfere, a texR poluprečnik kruga u teksturnim koordinatama koji se primenjuje na sferu
(vidi sliku).
void CGLRenderer::DrawSphere(double r,int nSeg, double texU,double texV, double texR)
{
int i, j;
double ang1, ang2;
double dAng1, dAng2;
dAng1 = PI /(double)nSeg;
dAng2 = 2 * PI / (double)nSeg;
ang1 = ‐PI / 2;
for (i = 0; i < nSeg; i++) {
ang2 = 0;
glBegin(GL_QUAD_STRIP);
for (j = 0; j < nSeg+1; j++) {
double x1 = r * cos(ang1) * cos(ang2);
double y1 = r * sin(ang1);
double z1 = r * cos(ang1) * sin(ang2);
double x2 = r * cos(ang1 + dAng1) * cos(ang2);
double y2 = r * sin(ang1 + dAng1);
double z2 = r * cos(ang1 + dAng1) * sin(ang2);
double tx1 = texR *x1 / r + texU;
double ty1 = texR *z1 / r + texV;
double tx2 = texR *x2 / r + texU;
double ty2 = texR *z2 / r + texV;
glTexCoord2f(tx1, ty1);
glVertex3d(x1, y1, z1);
glTexCoord2f(tx2, ty2);
glVertex3d(x2, y2, z2);
ang2 += dAng2;
}
glEnd();
*/
