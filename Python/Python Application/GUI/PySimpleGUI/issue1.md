
## Code

import PySimpleGUI as sg

from ImageOperator_class import ImageOperator

class ImageOperationWindow():
    def __init__(self):
        self.MainWindow()
        
    def MainWindow(self):
        
        src_img=ImageOperator(filepathname=None
                              ,need_check_filepathname_exists=False
                              ,need_prehandling_filepathname=False)
        dest_img=ImageOperator(filepathname=None
                              ,need_check_filepathname_exists=False
                              ,need_prehandling_filepathname=False)
        
        BROWSE = "Browse"
        OPEN = "Open"
        RESIZE = "Resize"
        TRANSPOSE="Transpose"
        ROTATE = "Rotate"
        
        MERGE = "Merge"
        
        
        image1Filepathname_key="-- Image 1 File Path Name--"
        image2Filepathname_key="-- Image 2 File Path Name--"
        
        browseButton1="Browse Button 1"
        browseButton2="Browse Button 2"
        
        openImageButton1="Open Image Button 1"
        openImageButton2="Open Image Button 2"
        
        resizeImageButton1="Resize Image Button 1"
        resizeImageButton2="Resize Image Button 2"
        
        transposeImageButton1="Transpose Image Button 1"
        transposeImageButton2="Transpose Image Button 2"
        
        rotateImageButton1="Rotate Image Button 1"
        rotateImageButton2="Rotate Image Button 1"
        
        flipImageButton1="Flip Image Button 1"
        flipImageButton2="Flip Image Button 2"
        
        scaleImageCombo1_key="--Scale Combo 1--"
        scaleImageCombo2_key="--Scale Combo 2--"
        
        transposeImageCombo1_key="--Transpose Combo 1--"
        transposeImageCombo2_key="--Transpose Combo 2--"
        
        rotateImageCombo1_key="--Rotate Combo 1--"
        rotateImageCombo2_key="--Rotate Combo 2--"
        
        mergeImagesButton="Merge Two Images Button"
    
        browseButton_dict={
            browseButton1:image1Filepathname_key
            ,browseButton2:image2Filepathname_key
        }
        
        openImageButton_dict={
            openImageButton1:[image1Filepathname_key,src_img]
            ,openImageButton2:[image2Filepathname_key,dest_img]
        }
        
        resizeImageButton_dict={
            resizeImageButton1:[image1Filepathname_key,src_img,scaleImageCombo1_key]
            ,resizeImageButton2:[image2Filepathname_key,dest_img,scaleImageCombo2_key]
        }
        
        transposeImageButton_dict={
            transposeImageButton1:[image1Filepathname_key,src_img,transposeImageCombo1_key]
            ,transposeImageButton2:[image2Filepathname_key,dest_img,transposeImageCombo2_key]
        }
        
        rotateImageButton_dict={
            rotateImageButton1:[image1Filepathname_key,src_img,rotateImageCombo1_key]
            ,rotateImageButton2:[image2Filepathname_key,dest_img,rotateImageCombo2_key]
        }
    
    
        availableExt=(('PNG', '.png'), ('JPG', '.jpg'))
        
        availableScale=[0.2
                        ,0.25
                        ,0.4
                        ,0.5
                        ,0.75
                        ,0.8
                        ,1
                        ,1.2
                        ,1.25
                        ,1.5
                        ,1.75
                        ,2
                        ,2.5
                        ,3
                        ,4
                        ,5
            ]
        
        availableTranspose=[0,1,2,3,4,5,6]
        
        availableDegree=[0,15,30,45,60,75,90,105,120,135,150,165,180,195,210,225,240,255,270,285,300,315,330,345]
        
        layout=[              
            [
                sg.Text("Image 1:"),
                sg.Input(key=image1Filepathname_key,readonly=True),
                sg.Button(key=browseButton1,button_text=BROWSE),
                sg.Button(key=openImageButton1,button_text=OPEN),
                sg.Combo(values=availableScale,default_value=1,key=scaleImageCombo1_key),
                sg.Button(key=resizeImageButton1,button_text=RESIZE),
                sg.Combo(values=availableTranspose,default_value=1,key=transposeImageCombo1_key),
                sg.Button(key=transposeImageButton1,button_text=TRANSPOSE),
                sg.Combo(values=availableDegree,default_value=100,key=rotateImageCombo1_key),
                sg.Button(key=rotateImageButton1,button_text=ROTATE)
                
            ]
            ,
            [
                sg.Text("Image 2:"),
                sg.Input(key=image2Filepathname_key,readonly=True),
                sg.Button(key=browseButton2,button_text=BROWSE),
                sg.Button(key=openImageButton2,button_text=OPEN),
                sg.Combo(values=availableScale,default_value=1,key=scaleImageCombo2_key),
                sg.Button(key=resizeImageButton2,button_text=RESIZE),
                sg.Combo(values=availableTranspose,default_value=1,key=transposeImageCombo2_key),
                sg.Button(key=transposeImageButton2,button_text=TRANSPOSE),
                sg.Combo(values=availableDegree,default_value=100,key=rotateImageCombo2_key),
                sg.Button(key=rotateImageButton2,button_text=ROTATE)
                
            ]
            ,
            [
                sg.Button(key=mergeImagesButton,button_text=MERGE)    
            ]
        ]
        window=sg.Window(title="Image Operation Window",layout=layout)
        
        
        while True:
            event,values=window.read()
            
            if event==sg.WIN_CLOSED:
                break
            
            if event in browseButton_dict.keys():
                filepathname=sg.popup_get_file(message="Select an image"
                                  ,title="Image Select Window"
                                  ,no_window=True
                                  ,file_types=availableExt
                                  )
                
                for k,v in browseButton_dict.items():
                    if event == k:
                        window[v].update(filepathname)
            
            if event in openImageButton_dict.keys():
                for k,v in openImageButton_dict.items():
                    if event == k:
                        val=values[v[0]]
                        print(val)
                        # Check if val is NOT empty
                        if (not val is None) and len(val)!=0:
                            v[1].SetFilepathname(val)
                            v[1].UploadImageWithFilepathname_Self()
                            v[1].OpenImage_Self()
                            
            if event in resizeImageButton_dict.keys():
                for k,v in resizeImageButton_dict.items():
                    if event == k:
                        val=values[v[0]]
                        scale=values[v[2]]
                        # Check if val is NOT empty
                        if (not val is None) and len(val)!=0:
                            v[1].SetFilepathname(val)
                            v[1].UploadImageWithFilepathname_Self()
                            v[1].ScaleImage_Self(scale)
                            #v[1].OpenImage_Self()
            
            if event in transposeImageButton_dict.keys():
                for k,v in transposeImageButton_dict.items():
                    if event == k:
                        val=values[v[0]]
                        degree=values[v[2]]
                        print("transposeImageButton_dict")
                        print(degree)
                        print(v[2])
                        print(k)
                        print(v)
                        # Check if val is NOT empty
                        if (not val is None) and len(val)!=0:
                            v[1].SetFilepathname(val)
                            v[1].UploadImageWithFilepathname_Self()
                            v[1].TransposeImage_Self(degree)
                            v[1].OpenImage_Self()
                            
                            
            if event in rotateImageButton_dict.keys():
                for k,v in rotateImageButton_dict.items():
                    if event == k:
                        val=values[v[0]]
                        degree=values[v[2]]
                        print("rotateImageButton_dict")
                        print(degree)
                        print(v[2])
                        print(k)
                        print(v)
                        # Check if val is NOT empty
                        if (not val is None) and len(val)!=0:
                            v[1].SetFilepathname(val)
                            v[1].UploadImageWithFilepathname_Self()
                            v[1].RotateImage_Self(degree)
                            v[1].OpenImage_Self()
                                          
            if event == mergeImagesButton:
                pass
            
            
            
        window.close()
    
def test():
    inst=ImageOperationWindow()
    
if __name__=='__main__':
    test()
    
## Expected Output
transposeImageButton_dict
2
--Transpose Combo 1--
Transpose Image Button 1
['-- Image 1 File Path Name--', <ImageOperator_class.ImageOperator object at 0x000001BA3F6737F0>, '--Transpose Combo 1--']
rotateImageButton_dict
100
--Rotate Combo 2--
Rotate Image Button 1
['-- Image 2 File Path Name--', <ImageOperator_class.ImageOperator object at 0x000001BA3F6732E0>, '--Rotate Combo 2--']
rotateImageButton_dict
100
--Rotate Combo 1--
Rotate Image Button 1
['-- Image 1 File Path Name--', <ImageOperator_class.ImageOperator object at 0x000001BA3F6732E0>, '--Rotate Combo 1--']


## Real Output
transposeImageButton_dict
2
--Transpose Combo 1--
Transpose Image Button 1
['-- Image 1 File Path Name--', <ImageOperator_class.ImageOperator object at 0x000001BA3F6737F0>, '--Transpose Combo 1--']
rotateImageButton_dict
100
--Rotate Combo 2--
Rotate Image Button 1
['-- Image 2 File Path Name--', <ImageOperator_class.ImageOperator object at 0x000001BA3F6732E0>, '--Rotate Combo 2--']
rotateImageButton_dict
100
--Rotate Combo 2--
Rotate Image Button 1
['-- Image 2 File Path Name--', <ImageOperator_class.ImageOperator object at 0x000001BA3F6732E0>, '--Rotate Combo 2--']
