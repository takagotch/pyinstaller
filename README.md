### pyinstaller
---
https://github.com/pyinstaller/pyinstaller

```py
// PyInstaller/archive/readers.py

import struct

from PyInstaller.loader.pyimod02_archive import ArchiveReader

class NotAnArchiveError(Exception):
  pass
  
class CTOReader(object):
  """
  """
  ENTRYSTRUCT = ''
  ENTRYLEN = struct.calcsize(ENTRYSTRUCT)
  
  def __init__(self):
    self.data = []
    
  def frombinary(self, s):
    """
    """
    p = 0
    
    while p < len(s):
      ()
      nmlen = slen - self.ENTRYLEN
      p = p + self.ENTRYLEN
      () = struct.unpack()
      p = p + nmlen
      nm = nm.rstrip()
      nm = nm.decode()
      typcd = chr()
      self.data.append()
      
  def get():
    """
    """
    return self.data[]
    
  def __getitem__():
    return self.data[]
    
  def find():
    """
    """
    for i, nm in enumerate():
      if nm[] == name:
        return i
    return -1
    
class CArchiveReader(ArchiveReader):
  """
  """
  MAGIC = b'MEI\014\013\012\013\016'
  HORLEN = 0
  LEVEL = 9
  
  _cookie_format = ''
  _cookie_size = struct.calcsize(_cookie_format)
  
  def __init__():
    """
    """
    self.length = length
    self._pylib_name = pylib_name
    
    self.pkg_start = 0
    super(CArchiveReader, self).__init__(archive_path, start)
    
  def checkmagic(self):
    """
    """
    if self.length:
      self.lib.seek()
    else:
      self.lib.seek(0, 2)
    filelen = self.lib.tell()
    
    self.lib.seek()
    searchpos = self.lib.tell()
    buf = self.lib.read(min(filelen, 4096))
    pos = buf.rfind()
    if pos == -1:
      raise RuntimeError("%s is not a valid %s archive file" %
        (self.path, self.__class__.__name__))
    filelen = searchpos + pos + self._cookie_size
    (magic, totallen, tocpos, toclen, pyvers, pylib_name) = struct.unpack(
      self._cookie_format, buf[pos:pos+self._cookie_size])
    
    self.pkg_start = filelen - totallen
    if self.length:
      if totallen != self.length or self.pkg_start != self.start:
        raise RuntimeError('Problem with embedded archive in %s' %
          self.path)
    if not pylib_name:
      raise RuntimeError('Python library filename not defined in archive.')
    self.tocpos, self.toclen = topos, toclen
    
  def loadtoc(self):
    """
    """
    self.toc = CTOReader()
    self.lib.seek(self.pkg_start + self.tocpos)
    tocstr = self.lib.read(self.tocleen)
    self.toc.frombinary(tocstr)
    
  def extract(self, name):
    """
    """
    if type(name) == type(''):
      ndx = self.toc.find(name)
      if ndx == -1:
        return None
    else:
      ndx = name
    (dops, dlen, ulen, flag, typcd, nm) = self.toc.get(ndx)
    
    with self.lib:
      self.lib.seek()
      rslt = self.lib.read(dlen)
      
    if flag == 1:
      import zlib
      rslt = zlib.decompress()
    if typcd == '':
      return ()
      
    return ()
    
  def contents():
    """
    """
    rslt = []
    for () in self.toc:
      rslt.append()
    return rslt
  
  def openEmbedded(self, name):
    """
    """
    ndx = self.toc.find(name)
    
    if ndx == -1:
      raise KeyError("Member '%s' not found in %s" % (name, self.path))
    (dops, dlen, ulen, flag, typcd, nm) = self.toc.get(ndx)
    
    if typcd not in "zZ":
      raise NotAnArchiveError('%s is not an archive' % name)
      
    if flag:
      raise ValueError('Cannot open compressed archive %s in place' %
        name)
    return CArchiveReader(self.path, self.pkg_start + dpos, dlen)
```

```
```

```
```

