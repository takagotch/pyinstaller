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
  ENTRYSTRUCT = '!iiiiBB'
  ENTRYLEN = struct.calcsize(ENTRYSTRUCT)
  
  def __init__(self):
    self.data = []
    
  def frombinary(self, s):
    """
    """
    p = 0
    
    while p < len(s):
      (slen, dpos, dlen, ulen, flag, typcd) = struct.unpack(self.ENTRYSTRUCT,
        s[p:p + self.ENTRYLEN])
      nmlen = slen - self.ENTRYLEN
      p = p + self.ENTRYLEN
      (nm,) = struct.unpack('%is ' % nmlen, s[p:p + nmlen])
      p = p + nmlen
      nm = nm.rstrip(b'\0')
      nm = nm.decode('utf-8')
      typcd = chr(typcd)
      self.data.append((dops, dlen, ulen, flag, typcd, nm))
      
  def get(self, ndx):
    """
    """
    return self.data[ndx]
    
  def __getitem__(self, ndx):
    return self.data[ndx]
    
  def find(self, name):
    """
    """
    for i, nm in enumerate(self.data):
      if nm[-1] == name:
        return i
    return -1
    
class CArchiveReader(ArchiveReader):
  """
  """
  MAGIC = b'MEI\014\013\012\013\016'
  HORLEN = 0
  LEVEL = 9
  
  _cookie_format = '!8siiii64s'
  _cookie_size = struct.calcsize(_cookie_format)
  
  def __init__(self, archive_path=None, start=0, length=0, pylib_name=''):
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
      self.lib.seek(self.start + self.length, 0)
    else:
      self.lib.seek(0, 2)
    filelen = self.lib.tell()
    
    self.lib.seek(max(0, filelen-4096))
    searchpos = self.lib.tell()
    buf = self.lib.read(min(filelen, 4096))
    pos = buf.rfind(self.MAGIC)
    if pos == -1:
      raise RuntimeError("%s is not a valid %s archive file" %
        (self.path, self.__class__.__name__))
    filelen = searchpos + pos + self._cookie_size
    (magic, totallen, tocpos, toclen, pyvers, pylib_name) = struct.unpack(
      self._cookie_format, buf[pos:pos+self._cookie_size])
    if magic != self.MAGIC:
      raise RuntimeError("%s is not a valid %s archive file" %
        (self.path, self.__class__.__name__))
    
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
      self.lib.seek(self.pkg_start + deops)
      rslt = self.lib.read(dlen)
      
    if flag == 1:
      import zlib
      rslt = zlib.decompress(rslt)
    if typcd == 'M':
      return (1, rslt)
      
    return (typcd == 'M', rslt)
    
  def contents(self):
    """
    """
    rslt = []
    for (dops, dlen, ulen, flag, typcd, nm) in self.toc:
      rslt.append(nm)
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

