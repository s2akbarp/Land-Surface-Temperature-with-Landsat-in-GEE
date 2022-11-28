# Land-Surface-Temperature-with-Landsat-in-GEE
This respositery provides the created codes for claculating LST using Landsat in GEE
LST is a controlling factor in water and energy exchange between the atmosphere and earth surface.
 Here, LST is evaluated using Landsat missions (Landsat 5 Thematic Mapper (TM), 7 Enhanced Thematic Mapper Plus (ETM+) and 8 Operational Land Imager (OLI) and Thermal Infrared Sensor (TIRS) data) for the SCRS. 
 In this study, the emissivity method (described in the Landsat Science Data Users Handbook) is implemented in the Google earth engine (GEE) to retrieve LST (Nasa;2012 n.d.).
 GEE is a platform for scientist analysis which provides a ready-to-use editor to test different algorithm and quickly see the results (Gorelick et al. 2017).
 The first step of the proposed method is converting the satellite-based digital number (DN) to at-sensor spectral radiance (〖 L〗_λ) (W·m−2·sr−1·µm−1) by equation (3.5) for Landsat5, 7 and  for Landsat8:
 
L_λ=[(〖L 〗_λmax  -〖L 〗_λmin)/(Q_CALmax-Q_CALmin  )][Q_CAL-Q_CALmin ]+〖L 〗_λmin                                       (6)
L_λ=  M_L*〖 Q〗_CAL+〖 A〗_L-〖 Q〗_I                                                                                                        


where Q_cal   is the quantized calibrated pixel value in DN, 〖L 〗_λmin   (W·m−2·sr−1·µm−1) represents the spectral radiance scaled to Q_CALMIN, 〖L 〗_λmax(W·m−2·sr−1·µm−1) is the spectral radiance scaled to〖 Q〗_CALMAX  .  Q_CALMIN  and 〖 Q〗_CALMAX are the minimum and maximum quantized calibrated pixels. In Equation (6),  M_L is Band specific multiplicative rescaling factor, O_i  is the correction for Band 10, and A_L is the Band-specific additive rescaling factor. All the values can be obtained from the metafiles of the images. Then, the TIRS band data is being converted from spectral radiance to at satellite brightness temperature by the Planck radiation function:
BT=k_2/(ln⁡[(k_1/〖 L〗_λ )+1])-273.15                


where K_1  (W·m^(-2)·sr^(-1)·µm^(-1) ) and K_2  (°C) represent the band-specific thermal conversion constants from the metadata, and the radiant temperature is converted to Celsius by adding absolute zero (approx. −273.15°C). Land surface emissivity (LSE) is the key parameter in calculating emissivity corrected LST. The NDVI Threshold (NDVITHM)-Based LSE Model is added to the main method for calculating LST (Li and Jiang 2018; Sekertekin and Bonafoni 2020). The surface emissivity was calculated from the following equations (Li and Jiang 2018; Sobrino et al. 2008): 
                                   NDVI=(NIR-RED)/(NIR+RED)                                                              
                                   
                              P_v=├ ((NDVI-NDVImin)/(NDVImax-NDVImin )┤)^2      
                              
                              
       ε_(λ=) {█(a_i ρ_R  +b_i                                        NDVI < 0.2
       ε_V λ P_v  +ε_S λ (1-p_v )+dε,dε=(1-ε_s  )(1-p_v )Fε_v       0.2 ≤ NDVI ≤ 0.5          (10)
       ε_V+dε                                                        NDVI > 0.5)┤         
where NIR is the reflection in the near-infrared spectrum, and RED is a reflection in the red range of the spectrum, P_v is the proportion of vegetation. In Equation (10), ρ_R is the reflectance value of the red band, a_i and b_i are estimated from an empirical relationship between the red band reflectance and Moderate Resolution Imaging Spectra radiometer emissivity library. F is a geometrical shape factor assumed with the value of 0.55 (Sobrino, Jiménez-Muñoz, and Paolini 2004), and〖 ε〗_v and ε_s   are the vegetation and soil emissivity, respectively, and d represents surface roughness (= 0 for homogenous and flat surfaces) taken as a constant value of 0.005. And, finally, T_s can be obtained by inverting Planck’s law as:
T_s   =k_2/(({1+[(λBT/ρ)lnε_λ  ]}))                                                                   (11)
 T_s is the LST in Celsius (°C), BT is at-sensor BT (°C), λ is the wavelength of emitted radiance, ε_λ is the emissivity, ρ=1.438*〖10〗^(-2)    derived from the Boltzmann and Planck’s constants. 


